# Forecast POC Guide

Amazon Forecast is a machine learning service that allows you to build and scale time series models in a quick and effective process. The content below is designed to help you build out your first models for your given use case and makes assumptions that your data may not yet be in an ideal format for Amazon Forecast to use.

This repository assumes a base familiarity with the service and if you have not already done so it is recommended that you use the getting-started material below.

## Introduction to Amazon Forecast

If you are not familiar with Amazon Forecast you can learn more about this tool on these pages:

* [Product Page](https://aws.amazon.com/forecast/)
* [GitHub Sample Notebooks](https://github.com/aws-samples/amazon-forecast-samples)
* [Product Docs](https://docs.aws.amazon.com/forecast/latest/dg/what-is-forecast.html)

## Completed Example

The notebooks have been scrubbed of all output before usage, however if you'd like to see a fully worked out example of this process, explore the notebooks in the `completed` folder.

## Process:

1. Deploying Your Working Environment
1. Validating and Importing Target Time Series Data
1. Validating and Importing Related Time Series Data (To Test)
1. Creating and Evaluating Your First Predictors
1. Importing Related Time Series Data (To Use)
1. Creating and Evaluating Related Time Series Enabled Predictors
1. Next Steps

That is a genereal order to this proccess, however if you are operating this as an assisted 2 day on-site POC. It is recommended that you operate steps 2 and 4 beforehand. Once the related data has been successfully imported you can delete it so that it does not muddy the results from your first Predictor.


## Deploying Your Working Environment

As mentioned above, the first step is to deploy a CloudFormation template that will perform much of the initial setup work for you. In another browser window or tab, login to your AWS account. Once you have done that, open the link below in a new tab to start the process of deploying the items you need via CloudFormation.

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=ForecastDemo&templateURL=https://chriskingpartnershare.s3.amazonaws.com/ForecastPOC.yaml)

Follow along with the screenshots below if you have any questions about deploying the stack.

### Cloud Formation Wizard

Start by clicking `Next` at the bottom like this:

![StackWizard](static/imgs/img1.png)

In the next page you need to provide a unique S3 bucket name for your file storage, it is recommended to simply add your first name and last name to the end of the default option as shown below, after that update click `Next` again.

![StackWizard2](static/imgs/img2.png)

This page is a bit longer so scroll to the bottom to click `Next`.

![StackWizard3](static/imgs/img4.png)


Again scroll to the bottom, check the box to enable the template to create new IAM resources and then click `Create Stack`.

![StackWizard4](static/imgs/img5.png)

For a few minutes CloudFormation will be creating the resources described above on your behalf it will look like this while it is provisioning:

![StackWizard5](static/imgs/img6.png)

Once it has completed you'll see green text like below indicating that the work has been completed:

![StackWizard5](static/imgs/img7.png)

Now that you have your environment created, you need to save the name of your S3 bucket for future use, you can find it by clicking on the `Outputs` tab and then looking for the resource `S3Bucket`, once you find it copy and paste it to a text file for the time being.

![StackWizard5](static/imgs/img8.png)


## Validating and Importing Target Time Series Data

Open `Validating_and_Importing_Target_Time_Series_Data.ipynb` and follow along there.

Once this has completed you can move onto prepping your Related Time Series data though you may not want to actually delete it after the import completes. 
If the data resides within your DatasetGroup then models will use it automatically when you train them and you are not able to determine the impact of just your base time series data easily.

## Validating and Importing Related Time Series Data

Amazon Forecast can certainly generate predictions using only the target data but the real power of the service comes into play when adding related time series information to facilitate better understanding of external signals, as well as item metadata that allows DeepAR+ to make assumptions about how a time series may behave when missing chunks of information.

Open `Validating_and_Importing_Related_Time_Series_Data.ipynb` and follow along there to prepare the dataset for the POC/Amazon Forecast.

## Creating and Evaluating Your First Predictors

In Amazon Forecast a model that has been trained on your data is called a Predictor, the notebook below will guide you through using the data you imported earlier to build your first predictors. At the end there is a bonus bit on running AutoML to determine the best. This is advised to be done before going home for the day as the process will take a number of hours to complete.

Open `Creating_and_Evaluating_Predictors.ipynb` and follow along to build these Predictors and see their results.

## Importing Related Time Series Data

Upon completing the initial models with just the target time series data go back to `Validating_and_Importing_Related_Time_Series_Data.ipynb` and execute the import job again if you deleted it during your validation phase. Once the data has been imported you are ready to move onto the next session of building a model with the related data.

## Creating and Evaluating Related Time Series Enabled Predictors

During this section you'll only be creating new models with Prophet and DeepAR+ this is because they are the only algorithms to incorporate related time series into their forecasts at this time within the service. As existing algorithms are modified or new ones are introduced this section will expand to cover those.

To get started simply open `Creating_and_Evaluating_Related_Time_Predictors.ipynb` this will be the last section of the POC that is guided and the rest will be an exploratory analysis to determine the value of any improvements.

## Next Steps

The next step is to either compare the results from Forecast against a previous approach and determine which one is more performant, if Forecast is more performant or there is no existing system then the path to production is determining how to integrate the results with planners, analysts, or other software solutions. The data again is exportable in JSON or CSV so it is easy to develop automated procedures for this integration.