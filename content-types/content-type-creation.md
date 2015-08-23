
# Create a Content Type

## Content Type Creation Methods

* Using the user interface.
* Using the server side object model.
* Using the client side object model.
* Deploying a farm solution (feature) that installs the content type based on an XML definition file.

## Using Schema Development

1. Create a farm solution
2. Select __Add New Item__, choose __Content Type__ from the __Office/SharePoint__ category.
3. Select the type to inherit from.
4. Close any designer view of the content type.
5. Open the elements.xml file of the content type, and replace it with the following:


    <?xml version="1.0" encoding="utf-8"?>  
      <Elements xmlns="http://schemas.microsoft.com/sharepoint/">  

        <!-- Site Columns -->
        <Field ID="{00D74D75-B775-4222-967D-D26973B8C5DB}"
          Name="CustomField"
          DisplayName="Custom Field"
          Type="Text"/>  

        <Field ID="{C3CD561C-D714-42DE-8112-C830B4108CEF}"
          Name="CustomChoiceField"
          DisplayName="Custom Choice Field"
          Type="Choice">  
        <CHOICES>  
          <CHOICE>Choice 1</CHOICE>  
          <CHOICE>Choice 2</CHOICE>  
          <CHOICE>Choice 3</CHOICE>  
        </CHOICES>  
        </Field>

        <!-- Parent ContentType: Item (0x01) -->  
        <ContentType ID="0x0100CB3ADB9F2D014E3F920C47C0B14D25BD"
          Name="SchemaCT"
          Group="Custom Content Types"
          Description="My Content Type"
          Inherits="TRUE"
          Version="0">  
        <FieldRefs>  
          <FieldRef ID="{00D74D75-B775-4222-967D-D26973B8C5DB}" Name="Custom Field" DisplayName="Custom Field"/>  
          <FieldRef ID="{C3CD561C-D714-42DE-8112-C830B4108CEF}" Name="Custom Choice Field" DisplayName="Custom Choice Field"/>  
          <FieldRef ID="{1DAB9B48-2D1A-47b3-878C-8E84F0D211BA}" DisplayName="$Resources:core,Status;" Name="_Status"/>  
        </FieldRefs>  
      </ContentType>  
    </Elements>

## Using Server Side Object model

    public static readonly SPContentTypeId myContentTypeId = new SPContentTypeId("0x010100FA0963FA69A646AA916D2E41284FC9D9");

    SPContentType myContentType = web.ContentTypes[myContentTypeId];

    if (myContentType == null)
    {
      myContentType = new SPContentType(myContentTypeId,
        web.ContentTypes, "My Content Type");

      web.ContentTypes.Add(myContentType);
    }

    // Associate existing fields with content type
    SPField field = web.AvailableFields[MyFieldId];
    SPFieldLink fieldLink = new SPFieldLink(field);

    if (myContentType.FieldLinks[fieldLink.Id] == null)
    {
        myContentType.FieldLinks.Add(fieldLink);
    }

    myContentType.Update(true);

https://msdn.microsoft.com/en-us/library/ff798370.aspx
http://www.c-sharpcorner.com/UploadFile/40e97e/create-content-type-using-object-model95/

## Using Client Object model

    ContentTypeCollection contentTypes = web.ContentTypes;
    cc.Load(contentTypes);
    cc.ExecuteQuery();

    foreach (var item in contentTypes)
    {
      if (item.StringId == "0x0101009189AB5D3D2647B580F011DA2F356FB2")
        return;
    }

    // Create a Content Type Information object.
    ContentTypeCreationInformation newCt = new ContentTypeCreationInformation();
    // Set the name for the content type.
    newCt.Name = "Contoso Document";
    // Inherit from oob document - 0x0101 and assign.
    newCt.Id = "0x0101009189AB5D3D2647B580F011DA2F356FB2";
    // Set content type to be available from specific group.
    newCt.Group = "Contoso Content Types";
    // Create the content type.
    ContentType myContentType = contentTypes.Add(newCt);
    cc.ExecuteQuery();

    Using AddFieldAsXml you can add fields to the FieldCollection of a site collection:
    FieldCollection fields = web.Fields;
    cc.Load(fields);
    cc.ExecuteQuery();

    string FieldAsXML = @"<Field ID='{4F34B2ED-9CFF-4900-B091-4C0033F89944}' Name='ContosoString' DisplayName='Contoso String' Type='Text' Hidden='False' Group='Contoso Site Columns' Description='Contoso Text Field' />";
    Field fld = fields.AddFieldAsXml(FieldAsXML, true, AddFieldOptions.DefaultValue);
    cc.Load(fields);
    cc.Load(fld);
    cc.ExecuteQuery();

    FieldCollection fields = web.Fields;
    Field fld = fields.GetByInternalNameOrTitle("ContosoString");
    cc.Load(fields);
    cc.Load(fld);
    cc.ExecuteQuery();

    FieldLinkCollection refFields = myContentType.FieldLinks;
    cc.Load(refFields);
    cc.ExecuteQuery();

    foreach (var item in refFields)
    {
      if (item.Name == "ContosoString")
        return;
      }

    FieldLinkCreationInformation link = new FieldLinkCreationInformation();
    link.Field = fld;
    myContentType.FieldLinks.Add(link);
    myContentType.Update(true);
    cc.ExecuteQuery();

https://msdn.microsoft.com/en-us/library/office/mt203989.aspx
http://www.c-sharpcorner.com/Blogs/12115/how-to-create-a-site-content-type-using-csom-in-sharepoint-2.aspx
http://blog.mastykarz.nl/programmatically-creating-site-columns-content-types-app-model/
