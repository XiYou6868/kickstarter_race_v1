## Kickstarter Creator v4 USD 0422.xlsx (Creator Project Level Data)

### Unique Project Identifiers Consistency:
We confirm that each project's unique identifier (`ProjectID`) has remained consistent in both the March and April versions of the dataset.

### Data Deduplication:
This version of the dataset has undergone deduplication based on `CreatorName`, `ProjectName`, `Verified`, and `StartDate` to remove duplicate entries.

### Currency Conversion:
Unlike the previous version that assigned non-USD currency values to empty, this dataset converts `Pledged`, `Goal`, and `Amount` values from non-USD currencies to USD.

#### New Variable:
- **ProjectUSD**, **IsRewardUSD**, **IsPledgedGoalUSD** (bool): A binary indicator that helps identify whether a project on Kickstarter is denominated in US dollars (USD) or not. If `IsProjectUSD` is set to True, it means the project's monetary values either `Pledged`, `Goal`, or `RewardAmount` we collected from the Kickstarter platform is in USD. Conversely, if it is set to False, it indicates that the project uses a currency other than USD, and the monetary values have been converted to USD using the exchange rate provided by Kickstarter's platform on 2024-03-18.


### Creator Identification

The **'CreatorName'** is extracted from the project page on the Kickstarter platform. There are three types of names:

1. Only real person name, like: 'Matt Burks', "Nicholas Boggs & Joshua Unsderfer"
2. Real person name and organization name mixed, e.g. 'Clifton Broumand/Man & Machine, Inc.', 'Sam Kirby (Brisk Innovation, LLC)'
3. Only organization name, like: 'Big Kid Games', 'C-Force'

In the April version, creator identification heavily relied on the Flair NER library. 

- **ExtractedCreators**: List of individual creator names extracted from `CreatorName`  using the Flair NER library, providing a detailed view of project collaborators.
- **CoCreatorNum**: Number of co-creators involved, calculated from `ExtractedCreators`. For example, given the `CreatorName` "Nicholas Boggs & Joshua Unsderfer", with `ExtractedCreators` listed as "Nicholas Boggs, Joshua Unsderfer", the total number of creators is 2. Subtracting one to account for the primary creator, the `CoCreatorNum` would be 1, indicating that there is one co-creator in addition to the primary creator.
- **IsOrganization** and **Organization_Context**: Flags if the project creator `CreatorName` contains organizations based on keywords in `CreatorName`, with `Organization_Context` listing the matched keywords.
Example: If`CreatorName`  "Jane,XYZ Foundation", `IsOrganization` will be True, and `Organization_Context` will list "Foundation".
", indicating organizational backing.

Each individual creator extracted from the `CreatorName` field is referred to as a `SingleCreator`. These SingleCreators are assigned unique `CreatorID`s for individual analysis. 

The **CreatorID** column combines the`SingleCreator` name with the date they joined Kickstarter (`joined`), creating a unique identifier for each creator. The account name (`SingleCreator`) and joined time (`joined`) are keys. We can use **NoCreatorIdentified** is 'False' to filter the data with `CreatorID` is not empty.

For a project involving a `CreatorName` entry "Justin and Joel Johnson", with a join date of September 1, 2013, the respective CreatorIDs would be "Justin_20130901" and "Joel Johnson_20130901". Two separate rows are generated:
- **ProjectID**: 28, **SingleCreator**: "Justin", with a unique `CreatorID` "Justin_20130901"
- **ProjectID**: 28, **SingleCreator**: "Joel Johnson", with a unique `CreatorID` "Joel Johnson_20130901".
For only organization name,we the `ExtractedCreators` are empty,we do not have 
**NoCreatorIdentified**: We assign True to `NoCreatorIdentified` for rows where `CreatorID` is empty, indicating that no creator could be identified for those projects.

- **Project_CreatorID** (String): A unique identifier combining the project's ID and the creator's ID, facilitating the association between projects and their creators. We can use **NoCreatorIdentified** is 'False' to filter the data with `Project_CreatorID` is not empty.
- **IsFirstTimer**: A binary indicator derived from the creators' historical project data. It flags whether a `SingleCreator` is launching a Kickstarter project for the first time (True) or not (False) in our dataset, **EndDate** from January 1, 2015, to December 31, 2022, specifically within the 'Technology', 'Design', or 'Game' categories.
- **ProjectsByCreatorID**: This count metric tallies the total number of projects initiated by each `SingleCreator`  in our dataset,  **EndDate** from January 1, 2015, to December 31, 2022, specifically within the 'Technology', 'Design', or 'Game' categories.


### Race Prediction

The key difference between the April version and the previous one lies in the approach to creator identification and the treatment of single-word names.

In the April version, creator identification heavily relied on the Flair NER library ensuring more precise extraction of individual names from the 'CreatorName' field. In the previous version, we just used the  `CreatorName` and predicted the race and gender by packages, so the company name, real person name, and organization name mixed may be input and caused the inaccuracy.

For instance, consider the name "Kirby, SR". In the April version, "Sam Kirby" would be identified as the `SingleCreator`. While the previous version might treat "Sam" as the firstname `FirstName_new`and "SR" as the LastName `LastName_new`.

Additionally, single-word names like "John" were treated as just first names in the April version. This contrasts with the previous version, where they were interpreted as both first and last names, resulting in cases like "John John" for single-word inputs.

Furthermore, in the April version, we use multiple race prediction methods on both `SingleCreator` and `Verified`.`Verified` names were dissected into '`FirstNameVerified`, `LastNameVerified`  from the  `Verified`  field, along with `FirstName_new`, `LastName_new` from the `SingleCreator` field, to enable a nuanced examination.New race prediction methods using Python's ethnicolr and pyethnicity packages, along with R's predictrace package, applied to both `SingleCreator` and `Verified` names. This includes columns like `EthClrCreatorNameRace`, `PyEthCreatorNameRace`, `PrdTrcCreatorNameRace`, `MajorCreatorNameRace`, and their 'Verified' counterparts.

**IsIdentityVerified, IsVerifiedIDAvailable** (Boolean): Verification status of a Kickstarter account is verified or not,it the verified identity is available or not.
The column `PrdTrcCreatorNameRace` in the April version corresponds to `LiklyRaceLast` in version 0311.
Similarly, `MajorCreatorNameRace` in the April version aligns with `LiklyRaceMajority` in version 0311.
The column `PrdTrcCreatorNameGender` in the April version is referred to as `LiklyGender_new` in version 0311.
Additionally, `EthClrCreatorNameRace` in the April version is named `LiklyRacePredicted` in version 0311.
While the underlying logic might be similar, these changes indicate updates or refinements in the analysis process.



#### New Variables:
- **PrdTrcCreatorNameGender**
  - **Description:** Referred to **LiklyGender_new** in the previous version. Predicts the gender of the `FirstName_new` of `SingleCreator`  using R's predictrace package. The accuracy of the column would be approximately 88%.
  - **Categories:** 'male', 'female', 'NaN' (not specified).
  
- **PrdTrcVerifiedNameGender**
  - **Description:** Predicts the gender of the first name of `Verified` names using R's predictrace package. The accuracy of the column would be approximately 90%.
  - **Categories:** 'male', 'female', 'NaN' (not specified).
  
- **EthClrCreatorNameRace**
  - **Description:** Referred to the **LiklyRacePredicted** in the previous version. Predicts the racial background of the name listed under `FirstName_new`, `LastName_new` of the `SingleCreator`  using Python's ethnicolr package with both the first name and last name. The accuracy of the column would be approximately 72%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).

- **PyEthCreatorNameRace**
  - **Description:** Predicts the racial background of the name listed under `FirstName_new`, `LastName_new` of the `SingleCreator`  using Python's pyethnicity package with both the first name and last name. The accuracy of the column would be approximately 76%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white'.

- **PrdTrcCreatorNameRace**
  - **Description:** Referred to the **LiklyRaceLast** in the previous version. Predicts the racial background of the name listed under `FirstName_new`, `LastName_new` of the `SingleCreator` using R's predictrace package. If the prediction of the `LastName_new` using the lastname method of the package is 'NaN', we will take the value of the firstname method of the package of `FirstName_new`. The accuracy of the column would be approximately 80%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).

- **MajorCreatorNameRace**
  - **Description:** Referred to **LiklyRaceMajority** in the previous version. Predicts the major racial background of the name listed under `FirstName_new`, `LastName_new` of the `SingleCreator` (at least two methods agreed). The accuracy of the column would be approximately 90%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).

- **EthClrVerifiedNameRace**
  - **Description:** Predicts the racial background of FirstNameVerified`, `LastNameVerified` of the `Verified` names using Python's ethnicolr package. The accuracy of the column would be approximately 72%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).

- **PyEthVerifiedNameRace**
  - **Description:** Predicts the racial background of `FirstNameVerified`, `LastNameVerified` of the `Verified` names using Python's pyethnicity package. The accuracy of the column would be approximately 78%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white'.

- **PrdTrcVerifiedNameRace**
  - **Description:** Predicts the racial background of `FirstNameVerified`, `LastNameVerified` of the `Verified` names using R's predictrace package. The accuracy of the column would be approximately 80%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).

- **MajorVerifiedNameRace**
  - **Description:** Predicts the major racial background of `FirstNameVerified`, `LastNameVerified` of the `Verified` names (at least two methods agreed). The accuracy of the column would be approximately 90%.
  - **Categories:** 'asian', 'black', 'hispanic', 'white', 'NaN' (not specified).


### Time-Related Features:
- **YearStartDate**: Year extracted from the `StartDate`.
- **MonthStartDate**: Month extracted from the `StartDate`.
- **DayStartDate**: Day of the month extracted from the `StartDate`.
- **YearEndDate**: Year extracted from the `EndDate`.
- **MonthEndDate**: Month extracted from the `EndDate`.
- **DayEndDate**: Day of the month extracted from the `EndDate`.

### Location-Related Features:
- **ProLocationState**: State abbreviation extracted from `ProjectLocation`. Category: 50 U.S. states and the District of Columbia (DC).
- **CreatorLocationState**: State abbreviation extracted from `CreatorLocation`. Category: 50 U.S. states and the District of Columbia (DC) and countries.
- **IsLocationMatch**: Binary indicator (True/False) indicating whether the project and creator locations match.
- **IsDomesticCreator**: Binary indicator (True/False) indicating whether the creator is from the USA.

### Description-Related Features:

- **DescriptionLength**: Length of the project description.
- **Log_DescriptionLength**: Log transformation (log(x+1)) of the `DescriptionLength` column.
- **DescriptionSentiment**: Sentiment score of the project description .This score ranges from -1 (negative sentiment) to 1 (positive sentiment).
.
- **DescriptionClarity**: Readability score of the project description.This score is typically based on metrics like the Flesch-Kincaid Grade Level, with higher values indicating more difficult readability.

While the logic behind `DescriptionLength`, `DescriptionSentiment`, `DescriptionClarity`, and `Log_DescriptionLength` is similar in the previous version, they are not exactly identical. Since we clean text by removing punctuation, emojis, and other non-alphanumeric characters and generate them in the April version.

- **IsRaceMentioned**: Binary indicator (True/False) indicating whether race mentions are present in the project description.
- **KeywordOccurrences**: Count of keyword occurrences in the project description.


### Reward-Related Features:
- **CombinedRewardDescription**: Cleaned text of all types of rewards descriptions, derived from  `RewardDescript`  and `RewardName`  for each unique `RewardID` associated with a project.
- **AverageAmount**: Unweighted mean of the `Amount` column (average reward amount) for each project `ProjectID`.
- **AverageDeliveryTime**: Unweighted mean of the `DeliveryTime` column (average delivery time)for each project `ProjectID`.
- **AvailableRewards**: Count of available rewards where `Status` is 0 ("All gone")for each project `ProjectID`.
- **RewardDescriptionSentiment**: Sentiment polarity score (-1 to 1) for each cleaned `CombinedRewardDescription`.
- **RewardDescriptionClarity**: Flesch-Kincaid Grade Level score for each cleaned `CombinedRewardDescription`.
- **RewardDescriptionLength**: Word count for each cleaned `CombinedRewardDescription`.


### Revised Feature Descriptions

**IsFeatured** (Boolean): Indicates whether the project has been featured by Kickstarter, highlighting its prominence or appeal. The column name 'IsFeatured' was 'Featured' in version 0311.

**IsConnect** (Boolean): Indicates whether the creator has linked their project to social media platforms, enhancing visibility. The column name 'IsConnect' was 'Connect' in version 0311.

**WebNum** (Integer): Represents the number of web resources or external links provided by the creator, potentially indicating broader project support or references. The column name 'WebNum' was 'webNum' in version 0311.


### Detailed Variable Descriptions:

1. **Project_CreatorID** (String): A unique identifier combining the project's ID and the creator's ID.
2. **ProjectID** (Integer): Unique identifier for each project.
3. **CreatorID** (String): A unique identifier generated for each creator.
4. **SingleCreator** (String): Names of individual creators extracted from the original CreatorName field.
5. **CreatorName** (String): The name(s) of the project's creator(s) as listed on Kickstarter.
6. **Verified** (String): The name that the creator's identity has been verified by Kickstarter.
7. **State** (Boolean): Represents the project's success status.
8. **Total Pledges** (Float): Log-transformed total amount pledged for each project.
9. **Backer** (Float): The number of backers who have supported the project.
10. **Category** (String): The category under which the project is listed on Kickstarter.
11. **UpdateCount** (Integer): The number of updates posted by the creators during the campaign.
12. **CommentCount** (Integer): The number of comments on the project page.
13. **ProWeLove** (Boolean): Indicates whether the project has been flagged as a "Project We Love" by Kickstarter.
14. **Pledged** (Float): The total amount of money pledged to the project.
15. **Goal** (Float): The financial goal set by the project creators.
16. **IsFeatured** (Boolean): True if the project has been featured by Kickstarter.
17. **IsConnect** (Boolean): Indicates whether the creator has linked their project to social media platforms.
18. **IsIdentityVerified, IsVerifiedIDAvailable** (Boolean): Markers of identity verification.
19. **WebNum** (Integer): The number of web resources or external links provided by the creator.
20. **NoCreatorIdentified** (Boolean): Identifies whether the entry has a CreatorID or SingleCreator.
21. **IsFirstTimer** (Boolean): Identifies whether this is the creator's first Kickstarter project.
22. **ProjectsByCreatorID** (Float): Counts the total number of projects launched by each single creator.
23. **CoCreatorNum** (Integer): The count of co-creators involved in the project.
24. **IsOrganization** (Boolean): A True/False flag determined by the presence of organization-related keywords in the CreatorName.
25. **High_Educated_Bio** (Boolean): Indicates whether the creator's biography mentions a high level of education.
26. **Education_Level** (Category): Categories of the highest education level mentioned in the creator's biography.
27. **IsRaceMentioned** (Boolean): Flags whether the project's description includes keywords related to race or ethnicity.
28. **KeywordOccurrences** (Object): Details specific race-related keywords found in the project description.
29. **Days** (Integer): The total duration of the crowdfunding campaign.
30. **Time span** (Float): The log-transformed duration of the campaign in days.
31. **Financing goal** (Float): Log-transformed 'Goal' value.
32. **YearStartDate, MonthStartDate, DayStartDate, YearEndDate, MonthEndDate, DayEndDate** (int32): Breakdown of the project's start and end dates.
33. **ProLocationState and CreatorLocationState** (string): State abbreviations indicating the geographic origin of the project and its creator.
34. **IsLocationMatch** (bool): True if the project and creator share the same location.
35. **IsDomesticCreator** (bool): True if the creator is based in the United States.
36. **IsNameMatch** (bool): Indicates whether the 'Verified' and 'SingleCreator' entries share the same name.
37. **PrdTrcCreatorNameGender, PrdTrcVerifiedNameGender** (String): Predicted gender of the creator based on the first name.
38. **EthClrCreatorNameRace, PyEthCreatorNameRace, PrdTrcCreatorNameRace, MajorCreatorNameRace, EthClrVerifiedNameRace, PyEthVNameRace, PrdTrcVerifiedNameRace, MajorVerifiedNameRace** (String): Predicted race of the creator based on various methods.
39. **DescriptionLength, Log_DescriptionLength** (Integer, Float): Length and log-transformed length of the project description.
40. **DescriptionSentiment, DescriptionClarity** (Float): Sentiment and clarity scores of the project description.
41. **RewardNum, AverageAmount, AverageDeliveryTime, AvailableRewards** (Float, Float, Float, Integer): Details about project rewards.
42. **CombinedRewardDescription, RewardDescriptionSentiment, RewardDescriptionClarity, RewardDescriptionLength** (String, Float, Float, Integer): Details about project reward descriptions.
43. **IsProjectUSD, IsRewardUSD, IsPledgedGoalUSD** (bool): Flags indicating currency denomination.
44. **FirstNameVerified, LastNameVerified, FirstName_new, LastName_new** (String): Details about creator names.Verified` names were dissected into '`FirstNameVerified`, `LastNameVerified`  from the  `Verified`  field, along with `FirstName_new`, `LastName_new` from the `SingleCreator` field,
45. **newrace, verifiedrace, LiklyRace, LiklyGender_new, LiklyRaceVerified, LiklyGenderVerified** (String): intermedia result of the predicted race and gender details.


```python

```
