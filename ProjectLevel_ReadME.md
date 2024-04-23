## Kickstarter Project v4 USD 0422.xlsx (Project Level Data)

### Unique Project Identifiers Consistency:
We confirm that each project's unique identifier (`ProjectID`) has remained consistent in both the March and April versions of the dataset.

### Data Deduplication:
This version of the dataset has undergone deduplication based on `CreatorName`, `ProjectName`, `Verified`, and `StartDate` to remove duplicate entries.

### Currency Conversion:
Unlike the previous version that assigned non-USD currency values to empty, this dataset converts `Pledged`, `Goal`, and `Amount` values from non-USD currencies to USD.

#### New Variable:
- **IsProjectUSD**, **IsRewardUSD**, **IsPledgedGoalUSD** (bool): A binary indicator that helps identify whether a project on Kickstarter is denominated in US dollars (USD) or not. If `IsProjectUSD` is set to True, it means the project's monetary values either `Pledged`, `Goal`, or `RewardAmount` we collected from the Kickstarter platform is in USD. Conversely, if it is set to False, it indicates that the project uses a currency other than USD, and the monetary values have been converted to USD using the exchange rate provided by Kickstarter's platform on 2024-03-18.


### Creator Identification
We do not have the following columns in the dataset: `SingleCreator`, `CreatorID`, `Project_CreatorID`, `IsFirstTimer`, `NoCreatorIdentified`, `ProjectsByCreatorID`.
- **ProjectID**: 28, **CreatorName** : "Justin and Joel Johnson"




### Race Prediction
- **EthClrCreatorNameRace**
- **PyEthCreatorNameRace**
- **PrdTrcCreatorNameRace**
- **MajorCreatorNameRace**
- **EthClrVerifiedNameRace**
- **PyEthVerifiedNameRace**
- **PrdTrcVerifiedNameRace**
- **MajorVerifiedNameRace**


For each project,it contain all different kinds of races of `SingleCreator`.
Unique values in race prediction columns column:
- 'asian'
- 'black'
- 'hispanic'
- 'nan'
- 'white'
- 'asian, black'
- 'asian, hispanic'
- 'asian, nan'
- 'asian, white'
- 'nan, white'
- 'black, nan'
- 'black, hispanic'
- 'black, white'
- 'hispanic, nan'
- 'hispanic, white'
- 'asian, black, white'
- 'asian, hispanic, white'
- 'asian, nan, white'
- 'hispanic, nan, white'
and so on





### Gender Prediction
- **PrdTrcCreatorNameGender** 
- **PrdTrcVerifiedNameGender**
For each project,it contain all different kinds  of gender of `SingleCreator`.The gender prediction columnscontain the following categories:
Unique values in 'PrdTrcVerifiedNameGender' column:
- 'nan'
- 'male'
- 'female'
- 'female, male'
- 'female, male, nan'
- 'male, nan'
- 'female, nan'


### Detailed Variable Descriptions:
We drop the variables related to the **CreatorID** and the race predicted of the `SingleCreator`
1.	**ProjectID** (Integer): Unique identifier for each project.
2.	**CreatorName** (String): The name(s) of the project's creator(s) as listed on Kickstarter.
3.	**Verified** (String): The name that the creator's identity has been verified by Kickstarter.
4.	**State** (Boolean): Represents the project's success status.
5.	**Total Pledges** (Float): Log-transformed total amount pledged for each project.
6.	**Backer** (Float): The number of backers who have supported the project.
7.	**Category** (String): The category under which the project is listed on Kickstarter.
8.	**UpdateCount** (Integer): The number of updates posted by the creators during the campaign.
9.	**CommentCount** (Integer): The number of comments on the project page.
10.	**ProWeLove** (Boolean): Indicates whether the project has been flagged as a "Project We Love" by Kickstarter.
11.	**Pledged** (Float): The total amount of money pledged to the project.
12.	**Goal** (Float): The financial goal set by the project creators.
13.	**IsFeatured** (Boolean): True if the project has been featured by Kickstarter.
14.	**IsConnect** (Boolean): Indicates whether the creator has linked their project to social media platforms.
15.	**IsIdentityVerified, IsVerifiedIDAvailable** (Boolean): Markers of identity verification.
16.	**WebNum** (Integer): The number of web resources or external links provided by the creator.
17.	**NoCreatorIdentified** (Boolean): Identifies whether the entry has a CreatorID or SingleCreator.
18.	**CoCreatorNum** (Integer): The count of co-creators involved in the project.
19.	**IsOrganization** (Boolean): A True/False flag determined by the presence of organization-related keywords in the CreatorName.
20.	**High_Educated_Bio** (Boolean): Indicates whether the creator's biography mentions a high level of education.
21.	**Education_Level** (Category): Categories of the highest education level mentioned in the creator's biography.
22.	**IsRaceMentioned** (Boolean): Flags whether the project's description includes keywords related to race or ethnicity.
23.	**KeywordOccurrences** (Object): Details specific race-related keywords found in the project description.
24.	**Days** (Integer): The total duration of the crowdfunding campaign.
25.	**Time span** (Float): The log-transformed duration of the campaign in days.
26.	**Financing goal** (Float): Log-transformed 'Goal' value.
27.	**YearStartDate, MonthStartDate, DayStartDate, YearEndDate, MonthEndDate, DayEndDate** (int32): Breakdown of the project's start and end dates.
28.	**ProLocationState and CreatorLocationState** (string): State abbreviations indicating the geographic origin of the project and its creator.
29.	**IsLocationMatch** (bool): True if the project and creator share the same location.
30.	**IsDomesticCreator** (bool): True if the creator is based in the United States.
31.	**IsNameMatch** (bool): Indicates whether the 'Verified' and 'SingleCreator' entries share the same name in lower cases.
32.	**PrdTrcCreatorNameGender, PrdTrcVerifiedNameGender** (String): Predicted gender of the creator based on the first name.
33.	**EthClrCreatorNameRace, PyEthCreatorNameRace, PrdTrcCreatorNameRace, MajorCreatorNameRace, EthClrVerifiedNameRace, PyEthVNameRace, PrdTrcVerifiedNameRace, MajorVerifiedNameRace** (String): Predicted race of all the creators based on various methods.
34.	**DescriptionLength, Log_DescriptionLength** (Integer, Float): Length and log-transformed length of the project description.
35.	**DescriptionSentiment, DescriptionClarity** (Float): Sentiment and clarity scores of the project description.
36.	**RewardNum, AverageAmount, AverageDeliveryTime, AvailableRewards** (Float, Float, Float, Integer): Details about project rewards.
37.	**CombinedRewardDescription, RewardDescriptionSentiment, RewardDescriptionClarity, RewardDescriptionLength** (String, Float, Float, Integer): Details about project reward descriptions.
38.	**IsProjectUSD, IsRewardUSD, IsPledgedGoalUSD** (bool): Flags indicating currency denomination.
38. **FirstNameVerified,LastNameVerified** (String):`Verified` names were dissected into '`FirstNameVerified`, `LastNameVerified`  from the  `Verified`  field, 


```python

```
