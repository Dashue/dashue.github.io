---
layout: post
title: Dezerialize to dynamic dictionar
categories: Uncategorized
published: draft
---
dynamic body = DynamicDictionary.Create(JsonConvert.DeserializeObject<DynamicDictionary&gt;(new StreamReader(Request.Body).ReadToEnd()));

    var payload = result.Body.DeserializeJson<Dictionary<string, object&gt;&gt;();