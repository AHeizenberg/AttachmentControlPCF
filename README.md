# PowerApps Attachment Control PCF 📁

**Author:** Ahad Hamirani <br/>
**Version:** 1.2.1

## Prerequisites

Before using the Attachment Control PCF, ensure you have added the PCF to your environment and added it to a Canvas app. Follow the steps in this guide to get started:
- [How to Add a PCF to Your Environment](https://docs.microsoft.com/en-us/powerapps/developer/component-framework/import-custom-controls)
- [How to Add a PCF to a Canvas App](https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/add-custom-control)

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
- **NOTE:** Makers must set the appropriate input file type in order to ensure that the files are not getting mismatched. Default value is Base64 and I reccomed to leave that
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

Then you can simply access the files in the collection use code similar to below:
  ```PowerApps
FileName : ThisItem.Value.name
FileContent : ThisItem.Value.content
````

### How to add defualt files to the attachment control
- In PowerApps the most common document storage and management is done through SharePoint. In order to add SharePoint Files as the defaults of this control you can create a power automate flow that returns the base64 content of files. This flow can be called within PowerApps and the files can be stored into a collection that can be passed to the PCF.
- **Example:** Triggering a flow that returns a file's name and content and storing the response in a collection
````PowerApps
With(
    {w_flowResponse: testingbinarydata.Run()},
    ClearCollect(
        col_files,
        Table(
            {
                FileName: w_flowResponse.filename,
                FileContent: w_flowResponse.filecontent
            }
        )
    )
)
````
**Note**: The PCF CAN handle multiple default files being passed in as long as it is in the following schema:
````PowerApps
Table(
  {
  FileName: Sample.filename,
  FileContent: Sample.filecontent
  }
)
````


### How to reset the PCF
Since PCF Components are unable to use the PowerFx Reset Control we will have to use a toggle to "Reset" this PCF. 
