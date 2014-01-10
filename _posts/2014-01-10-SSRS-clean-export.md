---
title: SSRS clean table export
layout: default
category: Reporting
tagline: "the wrong tool for the job"
tags: [SSRS, XSLT, table, export, data]
---
## SSRS clean exports
If you use SSRS at all you have probably run into the situation where your users want a whiz-bang report but want to export a simple grid. I firmly believe that if all you want is a table of data there are much better ways to get it but unfortunatly often there is complex business logic in the reports or your users are not up to the task of writing their own SQL queries. Therefore sometimes you have to use SSRS as a data extraction tool.

### XSLT method
The first and most labor intensive method of exporting a clean table of data from a complex SSRS report is to create an xslt file and deploy it with the report. This is also the most flexible method but to my knowledge there is no way to generate one automatically. So you are stuck keeping a template and manually altering it for every report and every time you update your columns or groopings you will have to modify that xslt.

If you have pretty static reports and good consistency across developers then this can work. You direct your users to export as XML and the export will be transformed by the xslt into whatever you told it to in an excel xml file.

### Hidden tablix method
Alternativly you can hide a table at the bottom of the report that contains all the columns you want to export in a simple format. You can either prompt the user for which format they want or use a conditional statment like:
{% highlight ruby %}
=iif(Globals!RenderFormat.Name="EXCEL",false,true)
{% endhighlight %}

This method will allow you to do your maintenance in Visual Studio and not have to work in XML but it still requires extra steps.

### Make the renderer do it
If you don't use a lot of extra text boxes you can get pretty good output by adding a modified version of the Excel renderer to your SSRS server. To do this you need to add an entry in rsreportserver.config. This entry should be along side the normal Excel entry unless you really don't use your normal Excel export.
{% highlight ruby %}
<Extension Name="EXCEL (No Header)" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering">
    <OverrideNames>
        <Name Language="en-AU">Excel (No Header)</Name>
    </OverrideNames>
    <Configuration> 
        <DeviceInfo>
            <SimplePageHeaders>True</SimplePageHeaders> 
        </DeviceInfo> 
    </Configuration> 
</Extension>
{% endhighlight %}

Using this renderer configuration will drop the page headers and simplify the output. If you still have extranious entries you can hide them with the conditional hide by RenderFormat.

This method requires the least maintenance but it is the least flexible.
