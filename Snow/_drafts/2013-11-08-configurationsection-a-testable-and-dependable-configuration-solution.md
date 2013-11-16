---
layout: post
title: ConfigurationSection "A testable, dependable configuration solution"
categories: .Net
published: true
---
I wanted to share a way of handling application configuration that I haven¨t ecountered anyone else using. It leverages [ConfigurationSection](http://msdn.microsoft.com/en-us/library/system.configuration.configurationsection(v=vs.110).aspx) and gives you the possiblility to test drive your settings.

## The basic scenario
Given an App.Config file with the following contents. (<u>Make sure you align the type property to your solution</u>)

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

For demonstration purpose we read a number, a string and an enum. Let's go ahead and specify what will be our contract: IMyConfiguration.

	public interface IMyConfiguration
    {
        int Age { get; }
        string Name { get; }
        DayOfWeek DayOfBirth { get; }
    }

Heading back to the test we see that the only thing left to implement is the MyConfiguration class. Let's add the following implementation.

    public class MyConfiguration : ConfigurationSection, IMyConfiguration
    {
        private MyConfiguration()
        {
        }

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

We make the default constructor private to expose only one way of instantiating the class. That last step should leave you with a passing test.

###Enums
We noticed how clean the enumeration was supplied to us, no calls to TryParse needed. What's also nice is that we get descriptive feedback if we were to misstype the value of an enum. For example changing the DateOfBirth to Mnoday gives the following exception:

	The value of the property 'DayOfBirth' cannot be parsed. 
	The error is: The enumeration value must be one of the following: 
	Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday

###Required
We will also get exception for any missing properties that are marked as required.

	Required attribute 'Age' not found.

##Moving on to the slightly more advanced stuff

###Lists
	<MyConfiguration MyStrings="value1,value2,value3" />
Alot of converters
###Nested values
	<list>
		<add value1/>
		<add value2/>
	</list>

	<list>
		<listItem>
			<Value1>
			<value2>
		</listItem>
	</list>
