# Is the Marvel Era Over? A Data-Driven Profitability Analysis
- Team: Apurva Dange & Ankush Harishchandre 
- Course: TEM 505 Data Driven Decision Making | Arizona State University

> *"Everyone has an opinion about whether Marvel is declining. This project is about replacing that opinion with evidence."*

<p align="center">
  <img src="https://github.com/apurva-dange/data_driven_decision_making/blob/main/project_media/marvel.jpeg" 
       alt="Marvel Movies Data Analysis" width="60%" />
</p>

## 📌 What Is This Project?

This is a complete end-to-end data analysis project that investigates one of the most debated questions in the entertainment industry right now, has Marvel's era of dominance come to an end, and if so, what does the data actually say about why?

The project was assigned as a final exam challenge in TEM 505 by Dr. Steve T. Cho. The framing was realistic by him - you are a data analyst being asked a business question, and your job is to acquire the right data, apply appropriate statistical techniques, and produce a justified, evidence-based answer. The deliverable was not a summary of what happened. It was a decision-support system built on statistical evidence that studios, investors, and production strategists could actually act on. The entertainment industry context makes the problem accessible and engaging, but every technique used here, from time-series trend analysis to Welch's T-test to multiple linear regression with train-test validation, transfers directly to financial analysis, consumer behavior research, product performance forecasting, and investment decision-making in any industry.

**Full analytical pipeline covered in this project:** Data Acquisition and Metric Engineering → Exploratory Data Analysis and Trend Visualization → Pearson Correlation Analysis → Statistical Hypothesis Testing with Welch's T-test → Multiple Linear Regression Modeling → Model Validation and R-squared Interpretation → Business Interpretation and Strategic Recommendations

## 🎯 The Three Questions This Project Answers

The professor structured this challenge around three specific questions that escalate in analytical complexity. Getting all three right required a different technique at each stage, and the combination of all three is what makes this a complete analytical workflow rather than a simple descriptive exercise.

The first question asked what critical factors drive the financial success of Marvel movies. The second asked whether the Marvel Era is genuinely coming to an end or whether any apparent decline is just noise in a small dataset. The third asked us to build a predictive model that forecasts which future Marvel films will perform well, trained on historical data and validated on held-out records to demonstrate real forecasting reliability.

## 🔧 Tech Stack — Tools Used and Why

**Language:** Python 3

**Environment:** Jupyter Notebook

**Dataset:** Marvel\_Movies\_Dataset.csv from Kaggle

Each library in this project was chosen for a specific reason, not just because it was available.

**Pandas** handled all data loading, cleaning, transformation, and engineered metric calculation. It is the right tool for structured tabular data because it supports vectorized column operations, which made the profit, ROI, and numerical CinemaScore calculations clean and efficient without writing manual loops.

**Numpy** supported the mathematical operations underlying the statistical calculations, particularly the array-level arithmetic that Pandas operations call internally for the custom metric transformations.

**Matplotlib** was used for the time-series line plots that form the foundation of the trend analysis. It gives precise control over plot styling, which matters when you are trying to communicate a finding clearly to a non-technical business audience.

**Seaborn** was specifically chosen for the correlation matrix visualization because it generates annotated heatmaps automatically, where each cell displays both the color encoding and the numerical correlation coefficient. A raw numerical table of the same data is significantly harder to interpret at a glance, and in business settings where stakeholders need to absorb findings quickly, visual clarity is not cosmetic, it is functional.

**Scikit-learn** provided the multiple linear regression modeling framework and the train-test split for model validation. The reason we used Scikit-learn's train-test split rather than measuring model fit on the full training data is that fitting and evaluating on the same data will always overstate predictive accuracy. The split enforces intellectual honesty: the model is trained on one portion of the data and evaluated on records it has never seen, which is the only valid measure of whether it actually forecasts or just memorizes.

**SciPy** enabled the Welch's T-test for statistical hypothesis testing. SciPy's stats module implements the correct degrees-of-freedom adjustment that Welch's test requires, which the standard T-test does not do.

## Phase 1: Data Engineering — Building Metrics That Answer Business Questions

The raw dataset contains movie titles, release dates, box office gross figures, production budgets, CinemaScore letter grades, IMDb ratings, and Rotten Tomatoes critic scores. None of these variables alone answers the question "what drives success?" So the first task was engineering decision-useful metrics from the raw inputs.

**Profit** was calculated as worldwide gross minus production budget. Gross revenue is a genuinely misleading success metric on its own, and this is a point that gets glossed over in casual entertainment reporting. A film earning 500 million dollars on a 450 million dollar production budget is significantly less successful than a film earning 200 million on a 30 million dollar budget. Profit strips the headline number away and reveals the actual financial outcome, which is the number that studios, investors, and distributors care about.

**Return on Investment (ROI)** was calculated as profit divided by production budget, multiplied by 100. This metric enables fair comparison across films at very different budget scales, and it directly answers the question that capital allocators ask: for every dollar we committed to this production, how many dollars came back? A film with a 50 million dollar budget and a 150 million dollar profit has a 200% ROI. A film with a 250 million dollar budget and a 300 million dollar profit has a 20% ROI. Both "made money," but they are not remotely equivalent business outcomes.

**Numerical CinemaScore** was the most technically consequential transformation. CinemaScore grades are letter-based values (A+, A, A-, B+, B, and so on), which means they cannot enter a Pearson correlation calculation or a regression model without transformation, because both of those methods require continuous numerical inputs. We mapped each grade to a numerical value on a consistent scale so that A+ received the highest value and lower grades received proportionally lower values, making the scoring system mathematically comparable and usable as a predictor variable. This kind of ordinal encoding is one of the most common and important data preparation steps in business analytics, and it is the step that made it possible to quantify how much opening-night audience satisfaction actually drives financial outcomes.

## Phase 2: Exploratory Data Analysis — Understanding the Trend Before Testing It

The first analytical step was time-series visualization of Marvel's key performance metrics over the full history of the franchise. The reason we visualized before running any formal statistical tests is that looking at the trend first tells you what hypothesis is worth testing, in what direction, and on which variables. Running tests blindly without understanding the shape of your data is a reliable way to test the wrong thing.

<p align="center">
  <img src="PLACEHOLDER_GRAPH1_2008_2019_TREND" 
       alt="Marvel Performance Trend 2008 to 2019 — Consistent Growth" width="80%" />
</p>

The data from 2008 to 2019 tells a clear and consistent story across all three performance dimensions. Profits, audience ratings, and critic scores all trended upward steadily across this entire period. The underlying drivers visible in the data are strong narrative continuity across interconnected films, careful long-form character development that compounded audience investment over time, and the increasingly large crossover events, most significantly Avengers: Infinity War and Avengers: Endgame, that created cultural moments driving enormous ticket sales. From a portfolio investment perspective, Marvel films during this era behaved like stable, low-variance assets. Historical performance reliably predicted future performance, and brand association alone was a meaningful signal of financial outcome.

<p align="center">
  <img src="PLACEHOLDER_GRAPH2_POST2021_VOLATILITY" 
       alt="Marvel Performance Volatility Post 2021 — Increased Variance" width="80%" />
</p>

The post-2021 picture is structurally different in a way that matters for business planning. The trend line does not collapse entirely, but the variance around it increases dramatically. Some films still perform very well. Others underperform significantly. The slope of the profit trend flattens and in some periods reverses, and the scatter of individual data points around that slope is far wider than in the pre-2021 era. In business terms, this is the shift from a stable, predictable asset to a volatile one. And volatility in a franchise context carries a specific implication: brand association alone can no longer be relied upon to forecast financial outcomes, which means investment decisions based primarily on the Marvel name are meaningfully riskier than they were five years ago.

## Phase 3: Correlation Analysis — Finding What Actually Predicts Profit

Before building any predictive model, we used Pearson correlation to measure the linear relationship between profit and each candidate predictor variable. The reason you do correlation analysis before regression is that regression will technically accept any variables you feed into it, but building a model on variables with no meaningful linear relationship to the outcome produces a model that fits noise rather than signal. Correlation analysis is the filter that tells you which variables deserve to be in the model and which do not.

<p align="center">
  <img src="PLACEHOLDER_GRAPH4_CORRELATION_MATRIX" 
       alt="Pearson Correlation Matrix — All Marvel Movie Features" width="80%" />
</p>

The correlation matrix produced three findings that directly shaped both the model architecture and the business interpretation.

CinemaScore showed the strongest correlation with profit at 0.66. Opening-night audience reaction is the single most predictive observable metric for financial outcome among all variables in the dataset. The mechanism is not mysterious: positive opening-night word-of-mouth drives repeat viewings across subsequent weekends, creates organic social media coverage that no paid marketing campaign can replicate at equivalent cost, and increases downstream merchandise, digital rental, and streaming licensing revenue. Negative opening-night sentiment does the opposite, and the damage compounds across every revenue channel simultaneously.

IMDb user ratings showed the second strongest correlation with profit. This reinforces the CinemaScore finding from a different measurement angle: audience-driven sentiment, whether measured by opening-night grading or cumulative user ratings, consistently outperforms professionally generated sentiment as a financial predictor. Audiences are the paying customers. Their reaction is the signal that matters most.

Production budget showed the weakest correlation with profit among all variables tested. This is the finding with the most direct strategic implication because it challenges the assumption that spending more is a reliable path to earning more. The data says it is not. Higher budgets do not reliably produce proportionally higher returns, and the films with the strongest ROI in the dataset were not necessarily the ones with the largest production investments.

<p align="center">
  <img src="PLACEHOLDER_GRAPH5_BUDGET_PROFIT_SCATTER" 
       alt="Production Budget vs Profit Scatter — Weak Linear Relationship" width="80%" />
</p>

## Phase 4: Statistical Hypothesis Testing — Confirming the Decline Is Real

Observing a downward trend visually is suggestive but not conclusive. With a relatively small number of films in the post-2021 group, it is entirely possible that what looks like a declining trend is just natural variation across a limited sample. This is where formal hypothesis testing becomes essential, because anyone making a real business decision based on this analysis needs to know whether the apparent decline is a statistically real signal or noise.

We applied Welch's T-test to compare pre-2021 and post-2021 Marvel films across both profit and critic score.

The choice of Welch's T-test over a standard Student's T-test is a methodological decision worth explaining clearly. The standard T-test assumes that the two groups being compared have equal variance, meaning that the spread of values within each group is approximately the same. But as the trend visualization already showed, the post-2021 group has dramatically higher variance than the pre-2021 group. The pre-2021 era had consistently high performance with relatively low scatter. The post-2021 era has mixed performance with very high scatter. Applying a standard T-test to groups with unequal variance produces unreliable p-values that can lead to incorrect conclusions about significance. Welch's T-test relaxes the equal-variance assumption and adjusts the degrees of freedom to account for the difference, making it the statistically correct choice for this specific comparison.

<p align="center">
  <img src="PLACEHOLDER_GRAPH3_TTEST_RESULTS" 
       alt="Welch T-Test Results — Pre vs Post 2021 Comparison" width="80%" />
</p>

The results were clear on one dimension and appropriately nuanced on the other.

The p-value for critic score decline was 0.038, which falls below the conventional 0.05 significance threshold. This means the decline in critical reception since 2021 is statistically significant. It is not noise. It reflects a real and measurable shift in perceived quality that is unlikely to be explained by random variation across a small sample of films.

The profit decline did not reach statistical significance. This requires careful interpretation because it is easy to read this result as "profits are fine," which is not what the data says. The correct interpretation is that the post-2021 profit data contains too much variability, driven by the simultaneous presence of strong performers and significant underperformers, to conclude with confidence that the average has fallen in a statistically meaningful way. What it does confirm is significantly increased unpredictability, and elevated unpredictability is itself a form of heightened business risk that rational planners should respond to regardless of whether the average has technically declined.

## Phase 5: Predictive Modeling — Building a Forecasting Tool Studios Can Use

With the correlation structure mapped and the statistical tests complete, we built a multiple linear regression model to predict profit using the three variables that the correlation analysis identified as the strongest predictors: CinemaScore, critic score, and production budget.

Multiple linear regression was the right modeling approach here for several interconnected reasons. The correlation analysis indicated that the relationships between the predictors and profit are plausibly linear, which is the fundamental assumption that regression requires. All three predictor variables are continuous numerical values, which regression handles naturally and efficiently. And most importantly for a business audience, regression produces interpretable coefficients: you can not only predict an outcome but also explain exactly which variables are driving the prediction, by how much, and in what direction. A model that produces accurate predictions but cannot explain them is far harder to trust and act on, especially in high-stakes financial decision contexts.

The model was trained on a subset of the historical Marvel film data and evaluated on a held-out test set that the model had never seen during training. This train-test split is the standard that any production-grade predictive model should meet. Measuring model fit on the same data used for training will always produce an inflated accuracy estimate because the model has had the opportunity to memorize patterns specific to those records. Testing on held-out data gives you an honest estimate of how well the model would actually perform on new, unseen films.

<p align="center">
  <img src="PLACEHOLDER_GRAPH6_REGRESSION_PREDICTIONS" 
       alt="Regression Model Predicted vs Actual Profit Comparison" width="80%" />
</p>

**Model Performance: R-squared of 0.66**

The best regression model achieved an R-squared value of 0.66, meaning the model explains approximately 66% of the variation in Marvel film profitability using just three measurable input variables. In a domain where emotional resonance, cultural timing, narrative continuity, and audience mood contribute substantially to outcomes in ways that are genuinely difficult to quantify, explaining two-thirds of the profit variance with three observable metrics represents strong explanatory power. For context, R-squared values above 0.6 are considered robust in behavioral and entertainment economics research, where inherent human unpredictability makes high R-squared values structurally harder to achieve than in physical or engineering domains.

**What the model enables in practice:**

The model makes strategic budget allocation decisions more defensible by quantifying the sensitivity of returns to changes in spending. It enables early warning assessment when pre-release test screening scores and initial critic reviews are fed into the model before theatrical release commitments are finalized, generating a profit range that can inform go/no-go decisions while there is still time to act. It supports the decision between theatrical release and direct-to-streaming routing based on predicted audience reception. And it provides a principled, evidence-based framework for script investment and development decisions during production, before the majority of budget has been committed and is no longer recoverable.

## 📈 Summary of Key Findings

**Critical factors driving Marvel movie success:** Audience satisfaction as measured by opening-night CinemaScore is the strongest predictor of financial performance, with a correlation of 0.66 with profit. Critic score is the second most predictive variable, functioning as professional validation that shapes early media coverage and social sentiment. Production budget is the weakest predictor, meaning spending scale does not reliably determine financial outcome. The economic logic of Marvel filmmaking has always been grounded in emotional investment and narrative continuity, not in production expenditure, and the data confirms that.

**Is the Marvel Era ending?** The honest, data-supported answer is nuanced and the nuance is important. Critical reception has significantly declined since 2021 at a statistically confirmed level (p = 0.038), meaning the drop in perceived quality is real and not random. Financial performance has not declined in a statistically significant way, but it has become far more volatile and unpredictable, which represents an elevated business risk even if average profit has not yet fallen. Marvel has transitioned from a stable, low-variance investment to a high-variance one. Whether that constitutes the end of an era depends on whether the studio can stabilize the audience satisfaction metric that the model shows drives outcomes most powerfully.

**Can future success be predicted?** Yes, with meaningful reliability. The regression model explains 66% of profit variance using three pre-measurable inputs, demonstrating that success is not random and can be reasonably forecast if studios track the right indicators systematically. CinemaScore, critic reception, and budget are all measurable before or immediately after release, which makes proactive forecasting genuinely possible rather than purely retrospective.

## Strategic Implications

These findings matter well beyond Marvel and apply to any high-budget entertainment franchise, subscription service, or consumer brand that depends on sustained audience loyalty. The data describes a structural shift in entertainment economics: audiences are more informed, expectations are higher, loyalty is conditional on quality rather than guaranteed by brand recognition, and value is determined by emotional engagement rather than visual spectacle.

Studios that incorporate predictive modeling into their production workflows from the scripting phase through test screenings and before promotional budget allocation will consistently make better capital allocation decisions than those relying on brand name assurance alone. Measuring audience sentiment before release rather than only after it is not a luxury, it is a risk management practice that the data supports directly.

## Why Each Method Was Chosen

This project used five distinct analytical techniques. 

Time-series visualization was used first because understanding the shape and direction of a trend visually prevents you from running statistical tests in the wrong direction or on the wrong variables. It is the diagnostic step that makes everything else more targeted. Pearson correlation was used to measure linear relationships between candidate predictors and the outcome before committing to a regression model, which is standard exploratory practice that prevents overfitting by helping identify only the variables with genuine explanatory power. Welch's T-test was chosen specifically because the pre and post-2021 groups have unequal variance, making the equal-variance assumption of a standard Student's T-test statistically inappropriate and potentially misleading for this comparison. Multiple linear regression was selected because the predictor-outcome relationships were plausibly linear, all variables were continuous, and interpretability was a requirement for a business audience that needs to act on the findings. The train-test split from Scikit-learn was used to validate model performance on held-out data, which is the only intellectually honest measure of predictive accuracy and the standard that any model presented to a real decision-maker should meet.


## 🔗 Project Links

📄 [Full Project Report (Google Docs)](https://docs.google.com/document/d/1gBjZPqe5CVxWOxn_q2LkYqS2vCdgmH0_PDCxLFSrK2M/edit?usp=sharing)

💻 [Jupyter Notebook Code (Google Drive)](https://drive.google.com/file/d/1X0zywdmWVD-PE-vkvge__NtTZ6QrKiK-/view?usp=sharing)

📊 [Kaggle Dataset — Marvel Movies Dataset](https://www.kaggle.com/datasets/sarthakbharad/marvel-movies-dataset)

> *"The best business decisions are not made by the person with the strongest opinion. They are made by the person who knows how to ask the data the right question and interpret the answer honestly."*
