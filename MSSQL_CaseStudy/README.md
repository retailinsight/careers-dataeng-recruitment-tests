# Retail Insight MSSQL Case Study
This case study is intended to be used for candidates who have no / very little MSSQL experience and is used to gage whether they would be able to learn / pick up the skill.

## Instructions for candidate
Thank you for taking the time to complete this case study, please download and extract the attachment RetailInsight_CaseStudy.zip, once extracted open and follow the instructions in Retail_Insight_MSSQL_Case_Study.pdf.

When you have completed the case study please reply with a single file attached containing all your code and any supplementary comments / commentary.
## Build RetailInsight_CaseStudy.zip

### Create Retail_Insight_MSSQL_Case_Study.pdf
First convert `Retail_Insight_MSSQL_Case_Study.md` to a PDF, if you use VSCode you can use this [plugin](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf).

#### Using VSCode Markdown PDF to convert markdown to pdf
- Open `Retail_Insight_MSSQL_Case_Study.md`
- Press `F1` or `Ctrl+Shift+P`
- Type `export` and select `markdown-pdf: Export (pdf)`

### Create RetailInsight_CaseStudy.zip
To create the zip you can run the following:
```Powershell
$compress = @{
  Path = "Retail_Insight_MSSQL_Case_Study.pdf", "RetailInsight_CaseStudy_Data.csv", "RetailInsight_CaseStudy_Data_set2.csv"
  CompressionLevel = "Fastest"
  DestinationPath = " RetailInsight_CaseStudy.zip"
}
Compress-Archive @compress
```
