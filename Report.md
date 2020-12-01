
# Credit Model Development Briefing

## 1. Overall Introduction

###  *Pending*

## 2. Model Development 
### 2.1 Default Defination
For example: 

#### (i) the bank determins that the obligor is unlikely to pay its obligation to the bank in full, without recourse to actions by the bank such as the realization of collateral; or

#### (ii) the obligor is more than 90 days past due on principle or interest on any material obligation to the bank; or

#### (iii) any events of default stipulated in the respective loan agreement.
### 2.2 Scorecard Methodology
### 2.2.1 Synthetic Models
### 2.2.1.1 Quantitative Component (**60**%)
 - ### Data
    #### (i) Internal Data
    
    #### (ii) External Data
 -  ### Variable (for Banks)
    |Capital Adequacy  | Asset Quality| Earnings| Liquidity | Management | 
    | ------------- |:-------------:| -----:| ---: | ---: |
    | Tier 1 Capital/Tangible Assets     | Non Performing Loans / Total Loans | Return on Assets(Growth) | Net Loans and Leases/Total Deposit| Total Interest Expense/Total Interest Income
- ### Variable Binning
    Typically, in a scorecard building, transformation solves tow objectives. First, it breaks up the factors into a few different ranges(binning) with each range assigned a transformed values; second, the transformed values is assigned in a way that the variable is correlated with the default rate.
 -  ### Model
     Logistic Regression
 -  ### Evaluation
    AUC/ROC, Sensitivity, Benchmarking, etc..
- ### Transforming coefficients into a percentage
    (i) Intercept is not considered in weight normalization. This is because the quantitative component of the model focuses on generating a quantitative score that can rank order the default likelihood of the obligors, and the rank order is not impacted by the intercept.
    
    (ii) For the independent variables, the coefficient should have a positive correlation with default. In particular, if the independent variable is negatively correlated  with default, the quantile is reversed with 1 minus the original quantile.
### 2.2.1.2 Qualitative Component (**40**%)
-  ### Score
    |Category | Commonly used number of bins | Commonly used score allocation|
    | ---| ---| ---|
    |Qualitative Factors| 4 | 100(Excellent), 80(Moderate), 50(Fair), 0(Weak)|

-  ### Weights

### 2.2.1.3 Score Transformation（Central Tendency) 
-  ### Central Tendency Definition
    #### Typically, a CT represents a "through the cycle" PD, i.e. a stable non-cyclical default rate, and is determined by the average level of PDs within whole portfolio.
- Implied PD
  
  The Logistic regression is to first regress the final score against the default identifier, and then obtain the initial default probability: $$PD_{implied} = \frac{1}{1+exp(\alpha+\beta\times Total Score)}$$
  where:
$$Quantitative Score = \sum Quantile Bin  \times  100 \times Factor Weight$$ 
$$Qualitative Score = \sum Answer Score  \times Factor Weight$$ 
$$ Total Score = Quantitative Score \times 60\% + Qualitative Score \times 40\% $$

- K-Factor determination
  
  The formula for calculating the K factor:
  $$ K = \frac{\frac{1-CT}{CT}}{\frac{1-average(PD_{implied})}{average(PD_{implied}}}$$
Where CT represents the Central Tendency.

- Calculation of actual default probability
  
  The formula for calculating the actual default probability:
   $$PD_{calibrated} =\frac{1}{1+K\times exp(\alpha+\beta \times Score)}$$

### 2.2.1.4 Master Scale

Master scale is utilized in order to map PDs to internal rating scales. Technically, there exists separate and independent methodology for scale development. Further, once internal master scale is completed, it is of great use to benchmarking external ratings.
### 2.2.1.5 Structured Adjustment

Structured adjustment is intended to capture unexpected events or sudden drwadowns, such as COVOD-19, and should be applied to final rating by notching up and down. It should be noted that the rationales for those adjustments are fully validated and objective.

### 2.2.1.6 Manually Override

Unlike structured adjustment, overrides are conducted by loan/debt approvers instead of analysts.

### 2.2.2 Judgemental Model
The model developers design survey questionnaires and distribute to the working group mumbers to complete the survey independently. The model developers then calculate the average weights from the surveys across each risk factor. The following rules are considered when determining the final weights.
- Total weights add up to 100%.
- The weight of a single risk factor should be at least 2%. Otherwise, it should be considered to be merged with other factors.
- The total weight of a risk category should not exceed 25%(30%) unless it is justified to have a dominant effect on the overall score. 

The process for establishing variable weights is iterative based on model testing results, and combines input from developers' industry experience, assessment of benchmark models, and feedback from working group sessions. 

