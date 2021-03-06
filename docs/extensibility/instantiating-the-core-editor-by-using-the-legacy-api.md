---
title: "Instantiating the Core Editor By Using the Legacy API | Microsoft Docs"
ms.date: "11/04/2016"
ms.topic: "conceptual"
helpviewer_keywords:
  - "editors [Visual Studio SDK], legacy - instantiating editor"
ms.assetid: dda23b18-96ef-43c6-b0dc-06d15cbe5cbb
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
  - "vssdk"
---
# Instantiate the core editor by using the legacy API

The editor is responsible for text editing functions such as insertion, deletion, copy, and paste. It combines these functions with the functions provided by language services, such as text coloring, indentation, and IntelliSense statement completion.

You can instantiate an instance of the core editor in one of three ways:

- Explicitly create an instance of the core editor in a window.

- Provide an editor factory that returns an instance of the core editor

- Open a file from the project hierarchy.

The following sections discuss how to use the legacy API to instantiate the editor.

## Explicitly open a core editor instance

When explicitly obtaining an instance of the core editor:

- Obtain a <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextBuffer> to hold the document data object being edited.

- Create a line oriented representation of the document data object by creating an <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextLines> interface from the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextBuffer> interface.

- Set <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextLines> as the document data object for an instance of the default implementation of the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsCodeWindow> interface, using the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsCodeWindow.SetBuffer%2A> method.

   Host the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsCodeWindow> instance in a <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame> interface by using the <xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.CreateToolWindow%2A> method.

At this point, displaying the <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame> interface provides a window that contains an instance of the core editor.

However, this is not a very useful instance, because it does not have shortcut keys, or access to advanced features. To obtain access to shortcut keys and advanced features:

- Use the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextBuffer.SetLanguageServiceID%2A> method to associate a language service and the document data object that the editor uses.

- Either create your own shortcut keys, or use the system default by setting the <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame> objects display properties. To do this, call the <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame.SetGuidProperty%2A> method with the [__VSFPROPID.VSFPROPID_InheritKeyBindings](<xref:Microsoft.VisualStudio.Shell.Interop.__VSFPROPID.VSFPROPID_InheritKeyBindings>) property.

   To obtain and use non-standard shortcut keys, generate them using the *.vsct* file. For more information, see [Visual Studio command table (.vsct) files](../extensibility/internals/visual-studio-command-table-dot-vsct-files.md).

## How to use an editor factory to obtain the core editor

When implementing a core editor with an editor factory using the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method, follow all the steps outlined in the previous section to explicitly host an <xref:Microsoft.VisualStudio.TextManager.Interop.IVsCodeWindow> using an <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextBuffer> document data object, in an <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame> object.

To display the text, obtain a <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextView> interface from the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsCodeWindow> object and call the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method.

To provide a language service to the editor, call the <xref:Microsoft.VisualStudio.TextManager.Interop.IVsTextBuffer.SetLanguageServiceID%2A> method within the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method.

To obtain default shortcut keys, unlike the previous section, you use the command context returned by the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method when obtaining the core editor from the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method.

If the <xref:Microsoft.VisualStudio.Shell.Interop.IVsEditorFactory.CreateEditorInstance%2A> method returns the same command GUID as the text editor, the instance of the core editor automatically obtains the default shortcut keys.

For general information, see [Walkthrough: Create a core editor and registering an editor file type](../extensibility/walkthrough-creating-a-core-editor-and-registering-an-editor-file-type.md).

## See also

- [Inside the core editor](../extensibility/inside-the-core-editor.md)
- [Open and save project items](../extensibility/internals/opening-and-saving-project-items.md)
- [Walkthrough: Create a core editor and registering an editor file type](../extensibility/walkthrough-creating-a-core-editor-and-registering-an-editor-file-type.md)