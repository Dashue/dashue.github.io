---
layout: post
title: camelCase when json serializing from ProperCase
categories: .Net, Json.Net, NancyFx
published: true
---

---excerpt
Follow naming conventions by *camelCasing* when serializing *ProperCase*
---end

## Background
To set the scene: I have a backend written in C# and a front-end written in Javascript. C# proposes *ProperCase* and Javascript propses *camelCase* naming convention. Now the issue is that by default Json.Net serializes the data using the same casing as the type being serialized from, leaving us in a situation where we return *ProperCase* serialized data to a solution that throughout uses a different casing. To avoid having a solution mixing conventions and to avoid having to remember which portion uses which convention, we want to serialize to the expected one.

## Solution
As with any problems there are many solutions, ranging from putting json attributes on your model specifying property names, creating a new model with propertynames following the expected covention (but breaking c# convention), using anonymous types with the right casing, or tweak your serialize.

I chose to tweak my serailizer to use 'camelCase' for all json data and as always with anything NancyFx related, the solution was very simple (2 steps).

**1** Create a customized serializer:

    public class CustomJsonSerializer : JsonSerializer
    {
        public CustomJsonSerializer()
        {
            ContractResolver = new CamelCasePropertyNamesContractResolver();
        }
    }

**2** Register it in NancyFx using the Ioc container of your choice (This exmple uses Ninject).

    protected override void ConfigureApplicationContainer(IKernel kernel)
    {
        kernel.Bind<JsonSerializer>().To<CustomJsonSerializer>();
    }