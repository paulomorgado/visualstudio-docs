---
title: "CA1009: Declare event handlers correctly"
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
  - "CA1009"
  - "DeclareEventHandlersCorrectly"
helpviewer_keywords:
  - "CA1009"
  - "DeclareEventHandlersCorrectly"
ms.assetid: ab65c471-1449-49d2-9896-7b9af74284b4
author: gewarren
ms.author: gewarren
manager: jillfra
dev_langs:
 - CPP
 - CSharp
 - VB
ms.workload:
  - "multiple"
---
# CA1009: Declare event handlers correctly

|||
|-|-|
|TypeName|DeclareEventHandlersCorrectly|
|CheckId|CA1009|
|Category|Microsoft.Design|
|Breaking Change|Breaking|

## Cause
 A delegate that handles a public or protected event does not have the correct signature, return type, or parameter names.

## Rule description
 Event handler methods take two parameters. The first is of type <xref:System.Object?displayProperty=fullName> and is named 'sender'. This is the object that raised the event. The second parameter is of type <xref:System.EventArgs?displayProperty=fullName> and is named 'e'. This is the data that is associated with the event. For example, if the event is raised whenever a file is opened, the event data typically contains the name of the file.

 Event handler methods should not return a value. In the C# programming language, this is indicated by the return type `void`. An event handler can invoke multiple methods in multiple objects. If the methods were allowed to return a value, multiple return values would occur for each event, and only the value of the last method that was invoked would be available.

## How to fix violations
 To fix a violation of this rule, correct the signature, return type, or parameter names of the delegate. For details, see the following example.

## When to suppress warnings
 Do not suppress a warning from this rule.

## Example
 The following example shows a delegate that is suited to handling events. The methods that can be invoked by this event handler comply with the signature that is specified in the Design Guidelines. `AlarmEventHandler` is the type name of the delegate. `AlarmEventArgs` derives from the base class for event data, <xref:System.EventArgs>, and holds alarm event data.

 [!code-cpp[FxCop.Design.EventsTwoParams#1](../code-quality/codesnippet/CPP/ca1009-declare-event-handlers-correctly_1.cpp)]
 [!code-csharp[FxCop.Design.EventsTwoParams#1](../code-quality/codesnippet/CSharp/ca1009-declare-event-handlers-correctly_1.cs)]
 [!code-vb[FxCop.Design.EventsTwoParams#1](../code-quality/codesnippet/VisualBasic/ca1009-declare-event-handlers-correctly_1.vb)]

## Related rules
 [CA2109: Review visible event handlers](../code-quality/ca2109-review-visible-event-handlers.md)

## See also

- <xref:System.EventArgs?displayProperty=fullName>
- <xref:System.Object?displayProperty=fullName>
- [Handling and raising events](/dotnet/standard/events/index)