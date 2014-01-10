## SSRS clean exports
If you use SSRS at all you have probably run into the situation where your users want a whiz-bang report but want to export a simple grid. I firmly believe that if all you want is a table of data there are much better ways to get it but unfortunatly often there is complex business logic in the reports or your users are not up to the task of writing their own SQL queries. Therefore sometimes you have to use SSRS as a data extraction tool.

### XSLT
The first and most labor intensive method of exporting a clean table of data from a complex SSRS report is to create an xslt file and deploy it with the report. This is also the most flexible method but to my knowledge there is no way to generate one automatically. So you are stuck keeping a template and manually altering it for every report and every time you update your columns or groopings you will have to modify that xslt.

If you have pretty static reports and good consistency across developers then this can work. You direct your users to export as XML and the export will be transformed by the xslt into whatever you told it to in an excel xml file.
