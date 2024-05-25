---
title: Access Wideners
description: The perfect way to access to the Minecraft fields and methods
authors:
  - Binaris
---

::: warning

You should have the decompiled Minecraft source code to use Access Wideners.
Use the genSources task and refresh the Gradle project to get the decompiled Minecraft source code.

Access Wideners don't work for the source of mods.

:::


Access Widener is the way that you could access to Minecraft fields and methods. It is a JSON file that you could put in 
your resources folder. It is a way to access to the Minecraft fields and methods without using reflection. This is a good 
way to access to the Minecraft fields and methods instead of normal accessor mixins in just a few cases, this 
is also called AW.

You must use Access Wideners instead of Access mixins in the following cases:
- You need to access a (package-private) class for shadowing or accessing a field or method in a mixin.
- Override final methods or subclass in final classes.
- If you want to subclass a class that only has a private constructor.

## Registering Access Widener
Fabric Loader -> 0.8.0 or higher
Loom -> 0.2.7 or higher

## Formatting Access Widener
You will use a file to define the access changes in your mode, for this you should use the `.accesswidener` file extension.

Now you must start the file with the following code, namespace will usually be set to the word `named`:
Loom will remap the access widener for you into intermediary along your mod.

`accessWidener v2 <namespace>`

## Class access
Keep in mind that:
- Class names need to be separated with a **/** and not **.**
- If you're trying to access an inner class, you should use **$** instand of **/**
 
Class access can be changed by specifying the class name as named the mappings namespace defined in this header.

`<access>   class   <className>`
- access can be accessible or extendable

## Method access

It can be changed by specifying the class name, method name and method descriptor as named the mappings namespace
defined in this header.

`<access>   method   <className>   <methodName>   <methodDescriptor>`
- access can be accessible or extendable
- className is the owner class
- methodName is the method name
- methodDesc is the method descriptor

## Field access
It can be changed by specifying the class name, field name and field descriptor as named the mappings namespace
defined in this header.

`<access>   field   <className>   <fieldName>   <fieldDesc>`
- access can be accessible or mutable
- className is the owner class
- fieldName is the field name
- fieldDesc is the field descriptor

## All the access changes
### Extendable
Only you should use it if you want to extend a class or override a method.
- Classes are made public and final is removed
- Methods are made protected and final is removed

### Mutable
Only you should use it if you want to change the value of a final field.

If you want to make a private field accesible and mutable, you need to use two directives for each one.

- Fields have final removed

## File locations
You need to specify the access widener in your build.gradle and fabric.mod.json files.
It should be in the resources and needs to be included in the jar file.

With Loom 0.9 or higher:
```gradle
loom {
    accessWidenerPath = file("src/main/resources/modid_please_change_me.accesswidener")
}
```

With Loom 0.8 or lower:
```gradle
loom {
	accessWidener = file("src/main/resources/modid_please_change_me.accesswidener")
}
```

In the fabric.mod.json file:
```json
{
  "accessWidener" : "modid_please_change_me.accesswidener"
}
```

## Validating the Access Widener
On recent versions of Loom, you can run `gradlew validateAccessWidener` to validate all the access changes.

If something goes wrong, the error could be a little bit cryptic, for reading it, you could check in this example:

If you have a error specifying the field, the error won't say whether the name or the type is the problem, if it says that
can't find the field `fooI`, it could mean there is no field `foo` or the type isn't an `int (I)`.