# PowerApps Attachment Control PCF 📁

**Author:** Ahad Hamirani <br/>
**Version:** 1.0.0.1

## How to download this PCF
In order to download this PCF navigate to the managed solution zip file attached to this repo and simply donwload it. 
<br/>
💡**Tip:** Press `Control + Shift + S` on Windows or `Command + Shift + S` on Mac to download the raw file

## Prerequisites

Before using the Attachment Control PCF, ensure the following:
1. You have added the Attachment Control PCF Solution to your environment
2. You have added the PCF control to your Canvas App <br/>

**Helpful Documentation**: [How to Add a PCF to A Canvas App](https://docs.microsoft.com/en-us/powerapps/developer/component-framework/import-custom-controls)

## Properties 🚀

### Allowed File Types
- **Description:** Specify the types of files allowed for upload. Each record must contain a DisplayName that will be shown to users and a MIME FileType ([Common MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types))
- **Type:** Table
- **Example:** In order to allow Excel Workbooks, PDF's and JPEG's to be uploaded you would create the table below
  ```PowerApps
  Table(
      {
          DisplayName: ".xlsx",
          FileType: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
      },
      {
          DisplayName: ".pdf",
          FileType: "application/pdf"
      },
      {
          DisplayName: ".jpeg",
          FileType: "image/jpeg"
      }
  )
  ````
   
### Default Files
- **Description:** This property allows makers to pass in default files that will be shown in the attachment control.
- **NOTE:** Makers must set the appropriate input file type in order to ensure that the files are not getting mismatched. Default value is Base64 and I reccomed to leave it like that
- **Type:** Table
- **Example:**
  ```PowerApps
  Table(
    {
        FileName: "example.pdf",
        FileContent: "application/pdf"
    }
  )
  ````
### Theme
- **Description:** Apply a Fluent UI theme to this PCF. The setup is identical to adding Theme to creator kit components

### Max Number of Files
- **Description:** Set the maximum number of files that can be uploaded.
- **Type:** Number
- **Example:** 2

### Max File Size
- **Description:** Specify the maximum size for each file (in MB).
- **Type:** Number
- **Example:** 4

### Max Total Size
- **Description:** Specify the maximum total size for all uploaded files (in MB).
- **Type:** Number
- **Example:** 10

### File Container Height
- **Description:** Set the height of the file container in pixels. Min height is set to 200px
- **Type:** Number
- **Example:** 400

## Some common scenarios 📖

### How to access list of attached files:
In order to parse all files into a PowerApps collection you can use the code below: 
  ```PowerApps
Table(ParseJSON(AttachmentControl.Files))
````

Then you can simply access the files in the collection using code similar to below:
  ```PowerApps
FileName : ThisItem.Value.name
FileContent : ThisItem.Value.content
````

### How to add defualt files to the attachment control
- In PowerApps the most common document storage and management is done through SharePoint. In order to add SharePoint Files as the defaults of this control you can create a power automate flow that returns the base64 content of files. This flow can be called within PowerApps and the files can be stored into a collection that can be passed to the PCF.
- **Example:** Triggering a flow that returns a file's name and content and storing the response in a collection
````PowerApps
With(
    {w_flowResponse: getFiles.Run()},
    ClearCollect(
        col_DefaultFiles,
        Table(
            {
                FileName: w_flowResponse.filename,
                FileContent: w_flowResponse.filecontent
            }
        )
    )
)
````
<br/> 🗒️**Note:** The PCF CAN handle multiple default files being passed in as long as the table follows the schema below:
````PowerApps
Table(
  {
  FileName: Sample.filename,
  FileContent: Sample.filecontent
  }
)
````
### How to reset the PCF
Since PCF Components are unable to use the PowerFx Reset Function we will have to use a toggle to "Reset" this PCF. 

**Purpose:** The reason I have added a reset toggle is to provide makers the ability to "reset" this PCF to it's `Default State.` <br/><br/>
**Default State:** The `Default State` of this PCF is one where all user uploaded files are cleared and any default provided files are kept or restored. <br/> <br/>
**Uses:** <br/>
**`Clear Files:`** This property can be used to clear all files within the attachment control (given that the default files are empty) <br/>
**`Restore Default Files:`** This property can restore all default files. This can be handy if makers use the attacment control as part of a form and want to restore changes <br/> <br/>
   🗒️**Note:** Setting this property to "true" will reset the control and keep it in reset mode, but in order to avoid any bugs please make sure to toggle the property off before using it again. <br/> <br/>
   💡**Suggestion:** I would reccomend setting this property to the inverse of the visibilty of the control. Meaning when the control is hidden it gets reset and when it is visible the reset property is toggled off.<br/>

## Conclusion
This is the intial version of this PCF and although I have used it in production and done testing it may contain bugs. If you encounter any issues or bugs feel free to let me know and I will try my best to address them. <br/>
Happy Developing!
