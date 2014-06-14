---
layout: post
title: Daily refactoring
categories: .Net
published: true
---
	#region MyRegion
		if (!MyHelper.MyBool)
    	{
    		myclass.Method(int, false, string, enum);
    		MyHelper.MyBool = true;
    	}
    	else
    	{
    		myclass.Method(int, true, string, enum);
    	}    
	#endregion    
Is semantically the same as:
	
	myClass.Method(int, MyHelper.MyBool, string, enum);
    MyHelper.MyBool = true;      
    
Tidier and faster : )

