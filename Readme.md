# Predicting Customer Dissatisfaction in Online Retail

## Overview

This project investigates how online retail transaction data can be transformed into a realistic predictive analytics problem for identifying orders that may result in customer dissatisfaction.

The project uses the Brazilian E-Commerce Public Dataset by Olist, which contains interconnected information about orders, products, payments, customers, sellers, and customer reviews.

Instead of using the customer review itself to predict dissatisfaction, the project defines a realistic prediction point at the time an order is handed over to the carrier. This ensures that the proposed predictive features represent information that could be available before the customer submits a review.

The project focuses on data integration, order-level feature engineering, target construction, data leakage prevention, missing-value handling, and exploratory data analysis.

---

## Project Title

**Predicting Customer Dissatisfaction in Online Retail: An Exploratory and Predictive Task Formulation Using Order-Level Data**

---

## Problem Statement

Online retailers need to identify potentially dissatisfied customers early enough to take corrective action.

However, customer review scores are only available after an order has been delivered and reviewed. Using the review itself or information generated after the review to predict dissatisfaction would create data leakage and would not provide operational value.

Therefore, this project investigates whether information available before the customer review can be transformed into a realistic prediction task for identifying orders that are likely to receive a low review score.

---

## Research Objective

The primary objective is to develop and evaluate a realistic data foundation for predicting whether a delivered online-retail order will receive a low customer review score.

A low review is defined as a review score of 1 or 2 stars.

---

## Specific Objectives

- Define a business-relevant binary prediction target for customer dissatisfaction.
- Identify information available at the intended prediction point.
- Integrate relational order, item, payment, product, customer, and seller information.
- Transform multiple transactional tables into an order-level analytical dataset.
- Create meaningful aggregated predictors from one-to-many relationships.
- Remove variables that could introduce post-outcome information leakage.
- Handle missing numerical and categorical values.
- Explore the distribution of low reviews.
- Investigate relationships between low reviews and order characteristics.
- Identify limitations and requirements for a future predictive modelling stage.

---

## Research Questions

1. Can online-retail transaction data be formulated into a realistic prediction task for low customer review scores?

2. Which order, payment, product, and seller-related variables can be constructed before the review outcome is known?

3. How does order-level aggregation affect the representation of item-level and payment-level information?

4. What exploratory patterns are visible between low reviews and order characteristics such as price, number of items, installments, and product category?

5. What risks and limitations must be addressed before deploying a predictive model?

---

## Dataset

The project uses the **Brazilian E-Commerce Public Dataset by Olist**.

The dataset contains approximately 100,000 orders and provides information related to:

- Orders
- Products
- Payments
- Reviews
- Customers
- Sellers
- Order items
- Product categories

The original dataset is relational rather than a single flat table. Therefore, joining and aggregation are important parts of the project.

---

## Dataset Architecture

The notebook loads eight interconnected CSV tables:

- `orders.csv`
- `order_items.csv`
- `order_payments.csv`
- `order_review.csv`
- `products.csv`
- `product_category_name_translation.csv`
- `customers.csv`
- `sellers.csv`

### Dataset Tables

| Table | Role | Important Information |
|---|---|---|
| orders | Central order entity | Order status, purchase and delivery timestamps |
| order_items | Item-level detail | Product, seller, price and freight |
| order_payments | Payment detail | Payment value and installments |
| order_review | Outcome source | Review score and post-delivery review information |
| products | Product context | Product category and physical attributes |
| product_category_name_translation | Category translation | Portuguese-to-English category mapping |
| customers | Customer context | Customer identification and location |
| sellers | Seller context | Seller identification and location |

---

## Prediction Timing

A major methodological focus of this project is defining a realistic prediction point.

The assumed prediction point is when the order is handed over to the carrier for delivery.

At this point, information such as the following may be available:

- Order information
- Payment information
- Product characteristics
- Seller information
- Estimated delivery information

The actual customer review is not yet available.

Defining this prediction point helps ensure that a future model does not use information that would only become available after the outcome.

---

## Target Variable

The target variable is:

`low_review`

The target is defined as follows:

- `1` = Review score ≤ 2
- `0` = Review score > 2

This converts the original review score into a binary classification target representing customer dissatisfaction.

### Target Interpretation

| low_review | Meaning |
|---:|---|
| 0 | Review score above 2 stars |
| 1 | Review score of 1 or 2 stars |

Low reviews represent clearly negative customer experiences and provide a business-oriented prediction target.

---

## Data Preparation

The notebook follows a structured data-preparation pipeline.

The main stages are:

1. Load the eight relational datasets.
2. Inspect their structure.
3. Filter the population to delivered orders.
4. Construct the `low_review` target.
5. Aggregate item-level information to the order level.
6. Aggregate payment information to the order level.
7. Incorporate product-category information.
8. Remove leakage-prone variables.
9. Handle missing values.
10. Perform exploratory data analysis.

---

## Order-Level Aggregation

The original data contains multiple records for some orders.

For example:

- One order can contain multiple products.
- One order can contain multiple order items.
- One order can contain multiple payment records.

To create a suitable analytical dataset, these one-to-many relationships are aggregated to the order level.

This creates a single analytical representation for each order.

---

## Feature Engineering

The project creates several order-level variables from the relational tables.

### Monetary Features

Examples include:

- `total_price`
- Freight or shipping-related values

These variables represent the monetary characteristics of an order.

### Order Structure Features

Example:

- `num_items`

This represents the size and complexity of an order.

### Payment Features

Payment-related information includes:

- Payment value
- Number of installments

These variables represent aspects of customer payment behaviour.

### Product Features

Product information is incorporated through product-category aggregation.

The representative product category is selected from the products associated with an order.

---

## Data Leakage Prevention

Preventing data leakage is one of the most important methodological components of this project.

Information that becomes available only after the customer review should not be used as a predictive feature.

Therefore, post-outcome information such as:

- Review text
- Review timestamps
- Other information generated after the outcome

is excluded from the predictive feature set.

This makes the proposed prediction problem more realistic and prevents future model evaluation from being artificially inflated.

---

## Missing-Value Handling

Missing values are handled according to variable type.

### Numerical Variables

Missing numerical values are intended to be replaced using the column median.

Median imputation provides a relatively robust approach when numerical variables contain extreme values.

### Categorical Variables

Remaining missing categorical values are replaced with:

`Unknown`

This preserves observations instead of unnecessarily removing rows.

---

## Exploratory Data Analysis

The project performs exploratory analysis to investigate relationships between order characteristics and customer dissatisfaction.

The analysis includes:

- Target distribution
- Total price analysis
- Number of items analysis
- Payment installment analysis
- Product-category analysis

---

## Target Distribution

The distribution of `low_review` is visualized using a count plot.

The analysis identifies low reviews as a minority class.

This creates a class-imbalance consideration for any future classification model.

Therefore, a future model should not rely on accuracy alone when evaluating performance.

---

## Total Price and Low Reviews

The project compares `total_price` between:

- Low-review orders
- Non-low-review orders

A boxplot is used to investigate whether order value differs between the two outcome groups.

A relationship between order value and review outcome could make total price a potentially useful predictive feature.

However, a visual association does not establish causality.

---

## Number of Items and Low Reviews

The project compares `num_items` across low-review and non-low-review orders.

Order complexity may affect customer experience because larger or more complex orders can involve:

- More products
- Multiple sellers
- Additional logistical interactions

The analysis therefore provides an initial feasibility check for this feature.

---

## Payment Installments and Low Reviews

Payment installment behaviour is compared between low-review and non-low-review orders.

Installment behaviour may reflect purchasing patterns and financial commitment.

However, the observed relationship should be interpreted as an association rather than evidence that installment choice causes dissatisfaction.

---

## Product Categories and Low Reviews

The project visualizes the ten most frequent product categories and compares their distribution by low-review status.

This analysis can reveal whether dissatisfaction appears to be concentrated in particular product categories.

Such patterns could potentially support:

- Category-level modelling
- Seller monitoring
- Product quality investigations
- Quality-control activities

---

## Key Findings

The project establishes that online-retail transaction data can support a coherent prediction problem when the prediction point is clearly defined and post-outcome information is excluded.

The analysis demonstrates that:

- Relational e-commerce data can be transformed into an order-level analytical dataset.
- Low customer reviews can be formulated as a binary target.
- Item-level information can be aggregated into order-level features.
- Payment-level information can be aggregated into order-level features.
- Product-category information can be incorporated into the analytical dataset.
- Low reviews represent a minority class.
- Data leakage needs to be explicitly controlled.
- Order value, number of items, payment installments, and product categories can be explored as potential predictive variables.

---

## Important Methodological Note

The current notebook is primarily a **data preparation and exploratory analysis project**.

It does **not** contain a completed machine-learning modelling experiment.

Therefore, this project does not claim final:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Precision-Recall AUC
- Best-performing machine-learning model

These results would require a separate supervised-learning stage.

---

## Limitations

### Review Selection Bias

Only customers who submit reviews are represented in the target outcome.

This can create self-selection bias because customers who do not submit reviews are not represented.

### Subjective Reviews

Review scores are subjective and may reflect individual expectations rather than objective product or service quality.

### Order-Level Aggregation

Item-level information is aggregated to the order level.

This may hide dissatisfaction associated with a specific product within a multi-item order.

### Representative Product Category

Orders containing multiple product categories are simplified using a representative category.

This can result in information loss.

### Missing Operational Factors

The dataset does not capture every operational factor that may influence customer satisfaction.

Examples include:

- Internal logistics decisions
- Customer-service interactions
- Other operational processes

### Prediction Timing Assumption

The assumed prediction point at carrier handover simplifies the real-world operational environment.

### Class Imbalance

Low reviews represent a minority outcome, which may cause a future classifier to favour the majority class.

### No Completed Predictive Model

The current notebook performs data preparation and exploratory analysis but does not train or evaluate a final predictive model.

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- Data Cleaning
- Exploratory Data Analysis
- Feature Engineering
- Relational Data Integration
- Predictive Analytics

---

## Project Workflow

The complete workflow is:

Dataset Loading  
↓  
Data Inspection  
↓  
Delivered Order Filtering  
↓  
Target Construction  
↓  
Order-Level Aggregation  
↓  
Feature Engineering  
↓  
Leakage Removal  
↓  
Missing-Value Handling  
↓  
Exploratory Data Analysis  
↓  
Findings and Discussion  
↓  
Limitations  
↓  
Future Predictive Modelling

---

## Project Structure

Online-Retail-Customer-Review-Prediction/

├── Dataset/  
│   ├── orders.csv  
│   ├── order_items.csv  
│   ├── order_payments.csv  
│   ├── order_review.csv  
│   ├── products.csv  
│   ├── product_category_name_translation.csv  
│   ├── customers.csv  
│   └── sellers.csv  
│  
├── Images/  
│   └── Exploratory Analysis Visualizations  
│  
├── Models/  
│   └── Future Model Development  
│  
├── Notebooks/  
│   └── Online_Retail_Customer_Review_Prediction.ipynb  
│  
├── Reports/  
│   └── Online_Retail_Customer_Review_Prediction_Term_Paper.pdf  
│  
├── .gitignore  
├── LICENSE  
├── README.md  
└── requirements.txt

---

## How to Run

### Step 1: Clone the Repository

git clone https://github.com/samyupolice/Online-Retail-Customer-Review-Prediction.git

### Step 2: Navigate to the Project Directory

cd Online-Retail-Customer-Review-Prediction

### Step 3: Install Dependencies

pip install -r requirements.txt

### Step 4: Open the Notebook

Open the notebook located in:

Notebooks/Online_Retail_Customer_Review_Prediction.ipynb

Run the notebook cells to reproduce the data loading, preparation, feature engineering, target construction, leakage prevention, missing-value handling, exploratory analysis, and visualizations.

---

## Future Machine Learning Development

The prepared order-level dataset can be used as the foundation for a complete supervised-learning pipeline.

Potential baseline models include:

- Logistic Regression
- Decision Tree
- Random Forest
- Gradient Boosting

Because low reviews are a minority class, future model evaluation should consider:

- Precision
- Recall
- F1-score
- ROC-AUC
- Precision-Recall AUC

Accuracy should not be treated as the only evaluation metric.

---

## Future Improvements

Future work can include:

- Developing and comparing supervised machine-learning models.
- Applying class-weighting or resampling techniques.
- Using temporal train-test validation.
- Adding legitimate delivery-performance variables available before the prediction point.
- Investigating seller-level features.
- Investigating product-category features.
- Performing feature-importance analysis.
- Applying model explainability techniques.
- Developing customer-dissatisfaction risk bands.
- Translating model predictions into actionable customer-service or logistics interventions.

---

## Potential Business Applications

A future predictive system based on this project could potentially help online retailers with:

- Proactive customer-service intervention
- Seller-quality monitoring
- Logistics improvement
- Product-category monitoring
- Customer-experience management
- Identification of potentially dissatisfied orders

For example, high-risk orders could potentially be prioritized for proactive customer-service actions before dissatisfaction becomes a formal complaint.

---

## Academic Report

A detailed academic report is included in the `Reports/` folder.

The report covers:

- Research problem
- Research objectives
- Research questions
- Dataset and data architecture
- Prediction timing
- Research methodology
- Data preprocessing
- Feature engineering
- Exploratory data analysis
- Findings and discussion
- Limitations
- Recommendations
- Future work
- Conclusion

---

## Conclusion

This project demonstrates how a relational online-retail dataset can be transformed into a realistic predictive-analytics problem for customer dissatisfaction.

The analysis integrates eight interconnected datasets and converts item-level, payment-level, product, customer, and seller information into an order-level analytical representation.

A binary target called `low_review` is created to represent one- and two-star customer reviews.

An important contribution of the project is its explicit attention to prediction timing and data leakage. Review information generated after the outcome is excluded so that a future predictive model can rely only on information that could realistically be available before the customer submits a review.

The current notebook establishes a structured data-preparation and exploratory-analysis foundation. A future supervised-learning stage can build on this foundation to determine whether customer dissatisfaction can be predicted with sufficient reliability for practical business use.

---

## Disclaimer

This project was developed for academic and research purposes.

The exploratory relationships identified in this project should not be interpreted as causal relationships.

A future predictive model would require appropriate validation, leakage controls, imbalance handling, and business evaluation before being used in a real-world environment.

---

## Dataset Reference

Brazilian E-Commerce Public Dataset by Olist

https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

---

## Author

**Samyuktha Police**

Data Science | Machine Learning | Predictive Analytics
