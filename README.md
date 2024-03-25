I get this question very often, which is a simple ask: how can I upload a file from Power Apps to a SharePoint List Item or Document Library ?

So today, let's cover once and for all these three scenarios:
<h1>1- The PowerApps App</h1>
For this Demo, I will create a simple Power Apps Canvas Application.

I have put in place a simple demo app, (link at the end of the post), which covers the three scenarios.

More importantly, we will need to get the attach file control.
<h2>1.1 Connect any data source which has a file field,</h2>
For this, I will connect to a SharePoint Document library "<strong>Documents</strong>", in my dev site.<img class="aligncenter wp-image-101284 size-full" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-24-at-23.22.52.png" alt="" width="1064" height="1262" />
<h1>1.2 Add an Edit Form</h1>
<img class="aligncenter wp-image-101286 size-medium" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-24-at-23.33.15-300x235.png" alt="" width="300" height="235" />

From the Insert button, search for the Edit Form and insert it into your screen.
<h2>1.3 Make sure you add the "Attachment" field</h2>
Once prompted, connect your edit form to your SharePoint Custom List and select the Attachment field.

You should get the following:

<img class="aligncenter wp-image-101287 size-medium" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-24-at-23.32.48-222x300.png" alt="" width="222" height="300" />

&nbsp;
<h2>1.4 Copy the Attachment control only</h2>
The edit form will display different cards (one for each field).

Locate the attachment card, and take a copy of the attachment control.

Keep it on a separate screen (as shown in the image below)

<a href="https://samtech365.com/wp-content/uploads/2024/03/AttachmentControl.gif"><img class="aligncenter wp-image-101288 size-full" src="https://samtech365.com/wp-content/uploads/2024/03/AttachmentControl.gif" alt="" width="1920" height="1010" /></a>

&nbsp;

<hr />

<h1>2- SharePoint List Item</h1>
Now, let's look at how we can attach a file to a SharePoint List Item.
<h2>2.1 First, let's create a Power automate</h2>
The flow will be triggered by the PowerApps upload button, which passes the title and attachment.

You can pass on additional details if you want.

First, the sharepoint list item is created and the title assigned.

Next, using the newly created item's id, we will attach the file.

Please follow the steps in the image bellow.

<a href="https://samtech365.com/wp-content/uploads/2024/03/Flow.gif"><img class="aligncenter size-full wp-image-101290" src="https://samtech365.com/wp-content/uploads/2024/03/Flow.gif" alt="" width="1920" height="1010" /></a>
<h2>2.2 Next, the PowerApp screen</h2>
For the sake of this demo, I will create a simple Custom List in SharePoint and call it "<strong>Contractors</strong>".

<img class="aligncenter size-full wp-image-101283" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-24-at-23.12.32.png" alt="Contractor SharePoint List" width="1736" height="758" />

In the demo app, I have added two fields to capture the <strong>Title</strong> and <strong>Attachment</strong>.

<a href="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-25-at-00.43.22.png"><img class="aligncenter wp-image-101291 size-medium" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-25-at-00.43.22-171x300.png" alt="" width="171" height="300" /></a>

&nbsp;
<h2>2.3 Connect your Power Automate to Power Apps</h2>
Now, we are ready to add the power automate to the power apps.

From the left panel, add an existing Power Automate, select the Power Automate created in step 2.1 and add it to your app.

<a href="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-25-at-00.50.34.png"><img class="aligncenter wp-image-101294 size-medium" src="https://samtech365.com/wp-content/uploads/2024/03/Screenshot-2024-03-25-at-00.50.34-243x300.png" alt="" width="243" height="300" /></a>
<h2>2.4 The submit button</h2>
Now, let's trigger our power automate and pass the required parametres.

The OnSelect action fo the submit is as follow
<pre class="EnlighterJSRAW" data-enlighter-language="generic">AttachtoShpList.Run(
    txtTitle.Text,
    {
        file: {
            contentBytes: First(att_AttachToShpList.Attachments).Value,
            name: First(att_AttachToShpList.Attachments).Name
        }
    }
);
</pre>
Where,

<strong>txtTitle</strong>: is the title (textbox) in the app

<strong>att_AttacheToShpList</strong>: is the attachment control.

&nbsp;

Please note that we will need to add the file{} record in this exact format.

I deliberately picked the first item of the Attachments table from the att_AttachToShpList, assuming you will need just one attachment.

If you would like to upload all the files, you can use a ForAll loop and run X number calls to the power automate.

&nbsp;

<hr />

<h1>3- SharePoint Document Library</h1>
If you need to upload the file to a SharePoint Document library, the process is pretty much the same.

I will copy the screen to a different one and amend the Power Automate to upload the file to a library instead of a list.
<h2>3.1 The Power Automate</h2>
Our Power Automate will take the same parameters, Title and File.

The steps are slightly different.

We will need just one action, which is to create a file.

Select the library where you want your file to be stored and pass the filename and content.

You can add more steps to set other properties of the file if needed.

<a href="https://samtech365.com/wp-content/uploads/2024/03/ShpLibrary.gif"><img class="aligncenter size-full wp-image-101301" src="https://samtech365.com/wp-content/uploads/2024/03/ShpLibrary.gif" alt="" width="1920" height="1010" /></a>
<h2>3.2 The submit button</h2>
Basically, the call to the new power automate is fairly similar to the previous one, just make sure you add your new PowerAutomate to the Power App, and use the following code in your Submit button &gt; OnSelect event.
<pre class="EnlighterJSRAW" data-enlighter-language="generic">AttachtoShpLibrary.Run(
    txtTitle2.Text,
    {
        file: {
            contentBytes: First(att_AttachToShpLibrary.Attachments).Value,
            name: First(att_AttachToShpLibrary.Attachments).Name
        }
    }
);
</pre>
Where,

<strong>txtTitle2</strong>: is the title (textbox) in the app

<strong>att_AttacheToShpLibrary</strong>: is the attachment control.

&nbsp;

In a future post, I will cover how we can upload a file to dataverse table in a File column.

Enjoy 😍

&nbsp;
