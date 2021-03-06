# CreatePredictor<a name="API_CreatePredictor"></a>

Creates an Amazon Forecast predictor\.

In the request, you provide a dataset group and either specify an algorithm or let Amazon Forecast choose the algorithm for you using AutoML\. If you specify an algorithm, you also can override algorithm\-specific hyperparameters\.

Amazon Forecast uses the chosen algorithm to train a model using the latest version of the datasets in the specified dataset group\. The result is called a predictor\. You then generate a forecast using the [CreateForecast](API_CreateForecast.md) operation\.

After training a model, the `CreatePredictor` operation also evaluates it\. To see the evaluation metrics, use the [GetAccuracyMetrics](API_GetAccuracyMetrics.md) operation\. Always review the evaluation metrics before deciding to use the predictor to generate a forecast\.

Optionally, you can specify a featurization configuration to fill and aggregate the data fields in the `TARGET_TIME_SERIES` dataset to improve model training\. For more information, see [FeaturizationConfig](API_FeaturizationConfig.md)\.

For RELATED\_TIME\_SERIES datasets, `CreatePredictor` verifies that the `DataFrequency` specified when the dataset was created matches the `ForecastFrequency`\. TARGET\_TIME\_SERIES datasets don't have this restriction\. Amazon Forecast also verifies the delimiter and timestamp format\. For more information, see [Datasets and Dataset Groups](howitworks-datasets-groups.md)\.

 **AutoML** 

If you want Amazon Forecast to evaluate each algorithm and choose the one that minimizes the `objective function`, set `PerformAutoML` to `true`\. The `objective function` is defined as the mean of the weighted p10, p50, and p90 quantile losses\. For more information, see [EvaluationResult](API_EvaluationResult.md)\.

When AutoML is enabled, the following properties are disallowed:
+  `AlgorithmArn` 
+  `HPOConfig` 
+  `PerformHPO` 
+  `TrainingParameters` 

To get a list of all of your predictors, use the [ListPredictors](API_ListPredictors.md) operation\.

**Note**  
Before you can use the predictor to create a forecast, the `Status` of the predictor must be `ACTIVE`, signifying that training has completed\. To get the status, use the [DescribePredictor](API_DescribePredictor.md) operation\.

## Request Syntax<a name="API_CreatePredictor_RequestSyntax"></a>

```
{
   "[AlgorithmArn](#forecast-CreatePredictor-request-AlgorithmArn)": "string",
   "[EncryptionConfig](#forecast-CreatePredictor-request-EncryptionConfig)": { 
      "[KMSKeyArn](API_EncryptionConfig.md#forecast-Type-EncryptionConfig-KMSKeyArn)": "string",
      "[RoleArn](API_EncryptionConfig.md#forecast-Type-EncryptionConfig-RoleArn)": "string"
   },
   "[EvaluationParameters](#forecast-CreatePredictor-request-EvaluationParameters)": { 
      "[BackTestWindowOffset](API_EvaluationParameters.md#forecast-Type-EvaluationParameters-BackTestWindowOffset)": number,
      "[NumberOfBacktestWindows](API_EvaluationParameters.md#forecast-Type-EvaluationParameters-NumberOfBacktestWindows)": number
   },
   "[FeaturizationConfig](#forecast-CreatePredictor-request-FeaturizationConfig)": { 
      "[Featurizations](API_FeaturizationConfig.md#forecast-Type-FeaturizationConfig-Featurizations)": [ 
         { 
            "[AttributeName](API_Featurization.md#forecast-Type-Featurization-AttributeName)": "string",
            "[FeaturizationPipeline](API_Featurization.md#forecast-Type-Featurization-FeaturizationPipeline)": [ 
               { 
                  "[FeaturizationMethodName](API_FeaturizationMethod.md#forecast-Type-FeaturizationMethod-FeaturizationMethodName)": "string",
                  "[FeaturizationMethodParameters](API_FeaturizationMethod.md#forecast-Type-FeaturizationMethod-FeaturizationMethodParameters)": { 
                     "string" : "string" 
                  }
               }
            ]
         }
      ],
      "[ForecastDimensions](API_FeaturizationConfig.md#forecast-Type-FeaturizationConfig-ForecastDimensions)": [ "string" ],
      "[ForecastFrequency](API_FeaturizationConfig.md#forecast-Type-FeaturizationConfig-ForecastFrequency)": "string"
   },
   "[ForecastHorizon](#forecast-CreatePredictor-request-ForecastHorizon)": number,
   "[HPOConfig](#forecast-CreatePredictor-request-HPOConfig)": { 
      "[ParameterRanges](API_HyperParameterTuningJobConfig.md#forecast-Type-HyperParameterTuningJobConfig-ParameterRanges)": { 
         "[CategoricalParameterRanges](API_ParameterRanges.md#forecast-Type-ParameterRanges-CategoricalParameterRanges)": [ 
            { 
               "[Name](API_CategoricalParameterRange.md#forecast-Type-CategoricalParameterRange-Name)": "string",
               "[Values](API_CategoricalParameterRange.md#forecast-Type-CategoricalParameterRange-Values)": [ "string" ]
            }
         ],
         "[ContinuousParameterRanges](API_ParameterRanges.md#forecast-Type-ParameterRanges-ContinuousParameterRanges)": [ 
            { 
               "[MaxValue](API_ContinuousParameterRange.md#forecast-Type-ContinuousParameterRange-MaxValue)": number,
               "[MinValue](API_ContinuousParameterRange.md#forecast-Type-ContinuousParameterRange-MinValue)": number,
               "[Name](API_ContinuousParameterRange.md#forecast-Type-ContinuousParameterRange-Name)": "string",
               "[ScalingType](API_ContinuousParameterRange.md#forecast-Type-ContinuousParameterRange-ScalingType)": "string"
            }
         ],
         "[IntegerParameterRanges](API_ParameterRanges.md#forecast-Type-ParameterRanges-IntegerParameterRanges)": [ 
            { 
               "[MaxValue](API_IntegerParameterRange.md#forecast-Type-IntegerParameterRange-MaxValue)": number,
               "[MinValue](API_IntegerParameterRange.md#forecast-Type-IntegerParameterRange-MinValue)": number,
               "[Name](API_IntegerParameterRange.md#forecast-Type-IntegerParameterRange-Name)": "string",
               "[ScalingType](API_IntegerParameterRange.md#forecast-Type-IntegerParameterRange-ScalingType)": "string"
            }
         ]
      }
   },
   "[InputDataConfig](#forecast-CreatePredictor-request-InputDataConfig)": { 
      "[DatasetGroupArn](API_InputDataConfig.md#forecast-Type-InputDataConfig-DatasetGroupArn)": "string",
      "[SupplementaryFeatures](API_InputDataConfig.md#forecast-Type-InputDataConfig-SupplementaryFeatures)": [ 
         { 
            "[Name](API_SupplementaryFeature.md#forecast-Type-SupplementaryFeature-Name)": "string",
            "[Value](API_SupplementaryFeature.md#forecast-Type-SupplementaryFeature-Value)": "string"
         }
      ]
   },
   "[PerformAutoML](#forecast-CreatePredictor-request-PerformAutoML)": boolean,
   "[PerformHPO](#forecast-CreatePredictor-request-PerformHPO)": boolean,
   "[PredictorName](#forecast-CreatePredictor-request-PredictorName)": "string",
   "[TrainingParameters](#forecast-CreatePredictor-request-TrainingParameters)": { 
      "string" : "string" 
   }
}
```

## Request Parameters<a name="API_CreatePredictor_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [AlgorithmArn](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-AlgorithmArn"></a>
The Amazon Resource Name \(ARN\) of the algorithm to use for model training\. Required if `PerformAutoML` is not set to `true`\.  

**Supported algorithms:**
+  `arn:aws:forecast:::algorithm/ARIMA` 
+  `arn:aws:forecast:::algorithm/Deep_AR_Plus` 

  Supports hyperparameter optimization \(HPO\)
+  `arn:aws:forecast:::algorithm/ETS` 
+  `arn:aws:forecast:::algorithm/NPTS` 
+  `arn:aws:forecast:::algorithm/Prophet` 
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `^[a-zA-Z0-9\-\_\.\/\:]+$`   
Required: No

 ** [EncryptionConfig](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-EncryptionConfig"></a>
An AWS Key Management Service \(KMS\) key and the AWS Identity and Access Management \(IAM\) role that Amazon Forecast can assume to access the key\.  
Type: [EncryptionConfig](API_EncryptionConfig.md) object  
Required: No

 ** [EvaluationParameters](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-EvaluationParameters"></a>
Used to override the default evaluation parameters of the specified algorithm\. Amazon Forecast evaluates a predictor by splitting a dataset into training data and testing data\. The evaluation parameters define how to perform the split and the number of iterations\.  
Type: [EvaluationParameters](API_EvaluationParameters.md) object  
Required: No

 ** [FeaturizationConfig](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-FeaturizationConfig"></a>
The featurization configuration\.  
Type: [FeaturizationConfig](API_FeaturizationConfig.md) object  
Required: Yes

 ** [ForecastHorizon](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-ForecastHorizon"></a>
Specifies the number of time\-steps that the model is trained to predict\. The forecast horizon is also called the prediction length\.  
For example, if you configure a dataset for daily data collection \(using the `DataFrequency` parameter of the [CreateDataset](API_CreateDataset.md) operation\) and set the forecast horizon to 10, the model returns predictions for 10 days\.  
The maximum forecast horizon is the lesser of 500 time\-steps or 1/3 of the TARGET\_TIME\_SERIES dataset length\.  
Type: Integer  
Required: Yes

 ** [HPOConfig](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-HPOConfig"></a>
Provides hyperparameter override values for the algorithm\. If you don't provide this parameter, Amazon Forecast uses default values\. The individual algorithms specify which hyperparameters support hyperparameter optimization \(HPO\)\. For more information, see [Choosing an Amazon Forecast Algorithm](aws-forecast-choosing-recipes.md)\.  
If you included the `HPOConfig` object, you must set `PerformHPO` to true\.  
Type: [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object  
Required: No

 ** [InputDataConfig](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-InputDataConfig"></a>
Describes the dataset group that contains the data to use to train the predictor\.  
Type: [InputDataConfig](API_InputDataConfig.md) object  
Required: Yes

 ** [PerformAutoML](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-PerformAutoML"></a>
Whether to perform AutoML\. When Amazon Forecast performs AutoML, it evaluates the algorithms it provides and chooses the best algorithm and configuration for your training dataset\.  
The default value is `false`\. In this case, you are required to specify an algorithm\.  
Set `PerformAutoML` to `true` to have Amazon Forecast perform AutoML\. This is a good option if you aren't sure which algorithm is suitable for your training data\. In this case, `PerformHPO` must be false\.  
Type: Boolean  
Required: No

 ** [PerformHPO](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-PerformHPO"></a>
Whether to perform hyperparameter optimization \(HPO\)\. HPO finds optimal hyperparameter values for your training data\. The process of performing HPO is known as running a hyperparameter tuning job\.  
The default value is `false`\. In this case, Amazon Forecast uses default hyperparameter values from the chosen algorithm\.  
To override the default values, set `PerformHPO` to `true` and, optionally, supply the [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md) object\. The tuning job specifies a metric to optimize, which hyperparameters participate in tuning, and the valid range for each tunable hyperparameter\. In this case, you are required to specify an algorithm and `PerformAutoML` must be false\.  
The following algorithm supports HPO:  
+ DeepAR\+
Type: Boolean  
Required: No

 ** [PredictorName](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-PredictorName"></a>
A name for the predictor\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z][a-zA-Z0-9_]*`   
Required: Yes

 ** [TrainingParameters](#API_CreatePredictor_RequestSyntax) **   <a name="forecast-CreatePredictor-request-TrainingParameters"></a>
The hyperparameters to override for model training\. The hyperparameters that you can override are listed in the individual algorithms\. For the list of supported algorithms, see [Choosing an Amazon Forecast Algorithm](aws-forecast-choosing-recipes.md)\.  
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Key Pattern: `^[a-zA-Z0-9\-\_\.\/\[\]\,\\]+$`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `^[a-zA-Z0-9\-\_\.\/\[\]\,\"\\\s]+$`   
Required: No

## Response Syntax<a name="API_CreatePredictor_ResponseSyntax"></a>

```
{
   "[PredictorArn](#forecast-CreatePredictor-response-PredictorArn)": "string"
}
```

## Response Elements<a name="API_CreatePredictor_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [PredictorArn](#API_CreatePredictor_ResponseSyntax) **   <a name="forecast-CreatePredictor-response-PredictorArn"></a>
The Amazon Resource Name \(ARN\) of the predictor\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `^[a-zA-Z0-9\-\_\.\/\:]+$` 

## Errors<a name="API_CreatePredictor_Errors"></a>

 **InvalidInputException**   
We can't process the request because it includes an invalid value or a value that exceeds the valid range\.  
HTTP Status Code: 400

 **LimitExceededException**   
The limit on the number of resources per account has been exceeded\.  
HTTP Status Code: 400

 **ResourceAlreadyExistsException**   
There is already a resource with this name\. Try again with a different name\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The specified resource is in use\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
We can't find a resource with that Amazon Resource Name \(ARN\)\. Check the ARN and try again\.  
HTTP Status Code: 400

## See Also<a name="API_CreatePredictor_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/forecast-2018-06-26/CreatePredictor) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/forecast-2018-06-26/CreatePredictor) 