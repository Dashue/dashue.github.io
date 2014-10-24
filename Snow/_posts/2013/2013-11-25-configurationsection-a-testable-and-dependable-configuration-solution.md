---
layout: post
title: ConfigurationSection "A testable, dependable configuration solution"
categories: .Net
published: true
---

---excerpt
Most every application and service needs configuration of some sort. This post explains an approach that offers testability.
---end

I wanted to share a way of handling application configuration that have helped me alot. It leverages [ConfigurationSection](http://msdn.microsoft.com/en-us/library/system.configuration.configurationsection(v=vs.110).aspx) and gives you the possiblility to test-drive your application settings. Let's jump in!
## The basic scenario
Given an App.Config file with the following contents. (**Make sure you align the type property to your solution**)

	<configuration>
		<configSections>
	    	<section name="MyConfiguration" type="My.Namespace.MyConfiguration, My.Assembly"/>
		</configSections>
	  
		<MyConfiguration Age="25" Name="Name" DayOfBirth="Monday" />
	</configuration>

In tdd fashion, let's start out with the test. Normally I would add one property at a time to get a short red-green cycle, but for the sake of keeping down the size of this post we will add all of them at once.

	[Fact]
    public void ReadMyConfiguration()
    {
        IMyConfiguration configuration = MyConfiguration.Get();

        Assert.Equal(25, configuration.Age);
        Assert.Equal("Name", configuration.Name);
        Assert.Equal(DayOfWeek.Monday, configuration.DayOfBirth);
    }

For demonstration purpose we read a number, a string and an enumeration. Let's go ahead and specify what will be our contract: IMyConfiguration.

	public interface IMyConfiguration
    {
        int Age { get; }
        string Name { get; }
        DayOfWeek DayOfBirth { get; }

        IEnumerable<string> MyStrings { get; }
        List<ConnectionEntry> MyConnections { get; }
    }

Heading back to the test we see that the only thing left to implement is the MyConfiguration class. Let's add the following implementation.

    public class MyConfiguration : ConfigurationSection, IMyConfiguration
    {
        private MyConfiguration() {}

        public static IMyConfiguration Get()
        {
            return (IMyConfiguration)ConfigurationManager.GetSection("MyConfiguration");
        }

        [ConfigurationProperty("Age", IsRequired = true)]
        public int Age
        {
            get
            {
                return (int)this["Age"];
            }
        }

        [ConfigurationProperty("Name", IsRequired = true)]
        public string Name
        {
            get
            {
                return (string)this["Name"];
            }
        }

        [ConfigurationProperty("DayOfBirth", IsRequired = true)]
        public DayOfWeek DayOfBirth
        {
            get
            {
                return (DayOfWeek)this["DayOfBirth"];
            }
        }
    }

We make the default constructor private to expose only one way of instantiating the class. This step should leave you with a passing test.

##Enums
Did you notice how cleanly the enumeration was supplied to us, no explicit calls to TryParse needed. What's also nice is that we get descriptive feedback if we were to misstype the value of an enum. For example changing the DateOfBirth to be misspellt "Mnoday" gives the following exception:

	The value of the property 'DayOfBirth' cannot be parsed. 
	The error is: The enumeration value must be one of the following: 
	Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday

##Required
We will also be given an exception for any missing properties that are marked as required.

	Required attribute 'Age' not found.

##Moving on to the marginally more advanced stuff

##Multiple Values
Sometimes storing and fetching individual values are not enough and the usage of lists of values are required. Leveraging the configuration section approach makes it really simple.

The following is my preferred way of storing multiple values, it's quick and simple.

	<MyConfiguration MyStrings="value1,value2,value3" />

&nbsp;

    [ConfigurationProperty("MyStrings", IsRequired = true)]
	[TypeConverter(typeof(CommaDelimitedStringCollectionConverter))]
	public IEnumerable<string> MyStrings
    {
    	get
        {
        	return ((CommaDelimitedStringCollection)this["MyStrings"]).Cast<string>();
        }
	}
    
    Assert.Equal(3, configuration.MyStrings.Count());
    Assert.Equal("Value1", configuration.MyStrings.ElementAt(0));
    Assert.Equal("Value2", configuration.MyStrings.ElementAt(1));
    Assert.Equal("Value3", configuration.MyStrings.ElementAt(2));

A bit more advanced approach can be used when wanting to store a list of entries with multiple values. This is done accordingly:

    <Connections>
      <add Server="Server1" Port="80"/>
      <add Server="Server2" Port="88"/>
    </Connections>

&nbsp;

    [ConfigurationProperty("Connections", IsRequired = true)]
    public ConnectionCollection ConnectionCollection
    {
        get { return ((ConnectionCollection)this["Connections"]); }
    }

    public List<ConnectionEntry> MyConnections
    {
        get { return ConnectionCollection.Cast<ConnectionEntry>().ToList(); }
    }

    public class ConnectionCollection : ConfigurationElementCollection
    {
        protected override ConfigurationElement CreateNewElement()
        {
            return new ConnectionEntry();
        }

        protected override object GetElementKey(ConfigurationElement element)
        {
            return ((ConnectionEntry)element).Server;
        }
    }
    
    public class ConnectionEntry : ConfigurationElement
    {
        [ConfigurationProperty("Server", IsRequired = true)]
        public string Server
        {
            get { return (string)this["Server"]; }
        }

        [ConfigurationProperty("Port", IsRequired = true)]
        public int Port
        {
            get { return (int)this["Port"]; }
        }
    }

    Assert.Equal(2, configuration.MyConnections.Count);
    Assert.Equal("Server1", configuration.MyConnections[0].Server);
    Assert.Equal(80, configuration.MyConnections[0].Port);
    Assert.Equal("Server2", configuration.MyConn
    ections[1].Server);
    Assert.Equal(88, configuration.MyConnections[1].Port);

##TypeConverters
Did you notice the use of the TypeConverter of type **CommaDelimitedStringCollectionConverter** in the previous example? This is just one of many converters provided by the framework. The following is a list of all the converters I was able to find. I'll leave it up to you to tinker around and figure out which ones can be useful to your configurations.

- ArrayConverter
- BooleanConverter
- ByteConverter
- CharConverter
- CollectionConverter
- CommaDelimitedStringCollectionConverter
- ComponentConverter
- CultureInfoConverter
- DateTimeConverter
- DateTimeOffsetConverter
- DecimalConverter
- DoubleConverter
- EnumConverter
- ExpandableObjectConverter
- ExtendedProtectionPolicyTypeConverter
- GenericEnumConverter
- GuidConverter
- InfiniteIntConverter
- InfiniteTimeSpanConverter
- Int16Converter
- Int32Converter
- Int64Converter
- MultilineStringConverter
- NullableConverter
- ReferenceConverter
- SByteConverter
- SingleConverter
- StringConverter
- TimeSpanConverter
- TimeSpanMinutesConverter
- TimeSpanMinutesOrInfiniteConverter
- TimeSpanSecondsConverter
- TimeSpanSecondsOrInfiniteConverter
- TypeConverter
- TypeNameConverter
- UInt16Converter
- UInt32Converter
- UInt64Converter
- UriTypeConverter
- WhiteSpaceTrimStringConverter

[Source for this blog post can be found here](https://github.com/Dashue/Blogging/tree/master/ConfigurationSection_Testable_and_Maintainable)

Have fun!