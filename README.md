# Retail Basket Analysis

What products are most frequently purchased together and how can that insight improve product placement and promotional bundling?

This project applies market basket analysis to transactional retail data from a Megastore to uncover purchasing patterns across customer orders. The Apriori algorithm is used to identify frequent itemsets and generate association rules, ranked by support, confidence, and lift, to surface actionable product pairing strategies.

## Key Findings

- The strongest rules produced a **lift of 88.20** — meaning customers who purchased specific antecedent items were **88x more likely** to also purchase the consequent items compared to random chance
- Top rules achieved **100% confidence**, indicating that every time the antecedent items appeared in a transaction, the consequent items were purchased as well
- The highest-lift rules centered on **Bakelike Alarm Clock variants** (Red, Green, Ivory, Pink) co-purchased with **Plasters in Tin** (Spaceboy, Circus Parade) and **Charlotte Bag Dolly Girl Design** — suggesting thematic, gift-bundle, or aesthetic-driven purchasing behaviour
- The Apriori algorithm was run with a **minimum support threshold of 1%** (items appearing together in at least 1% of all transactions) and a **minimum lift threshold of 1.3** to filter for meaningful associations

> 💡 **Interpretation:** The high lift scores reflect tight product clusters that customers consistently buy as a set — likely due to thematic similarity or gift-bundling intent. These groupings are strong candidates for pre-built promotional bundles, co-located shelf placement, or "customers also bought" recommendation triggers.

### Example Top Rule

| | Items |
|---|---|
| **If a customer buys** | Alarm Clock Bakelike Red · Plasters in Tin Spaceboy · Alarm Clock Bakelike Ivory |
| **They also tend to buy** | Alarm Clock Bakelike Green · Charlotte Bag Dolly Girl Design · Plasters in Tin Circus Parade · Alarm Clock Bakelike Pink |
| **Support** | 0.011 (1.1% of transactions) |
| **Confidence** | 1.000 (100%) |
| **Lift** | 88.20 |

## Dataset

Megastore transactional retail dataset including order-level and customer-level fields:

| Field | Description |
|---|---|
| OrderID | Unique transaction identifier |
| ProductName | Item purchased |
| Region | Customer region (e.g. Northeast US) |
| Segment | Customer type (e.g. Corporate) |
| OrderPriority | Urgency level of order |
| CustomerSatisfaction | Post-purchase satisfaction rating |
| Discount | Whether a discount was applied |
| PaymentMethod | Payment type used |

Data was transactionalized by grouping all products purchased within each OrderID into a single basket, then encoded into a binary matrix using `TransactionEncoder` for Apriori input.

## Approach

Categorical variables were classified as ordinal (OrderPriority, CustomerSatisfaction) or nominal (Region, Segment) and encoded accordingly — ordinal encoding for ranked variables and one-hot encoding for unranked ones. Order data was then transactionalized by aggregating ProductName by OrderID into lists, then transformed into a basket matrix.

The Apriori algorithm was applied with `min_support=0.01` and association rules were generated using `metric="lift"` with `min_threshold=1.3`. Rules were sorted by lift in descending order to surface the strongest associations first.

## Business Recommendations

- **Pre-build bundles** around the highest-lift product clusters (e.g. Bakelike Alarm Clock sets + Plasters in Tin) for promotional pricing or gift packaging
- **Implement "Customers also bought" logic** using these rules to drive cross-sell recommendations at checkout or on product pages
- **Co-locate high-association items** in physical stores or on the same category page online to encourage add-on purchases and increase average order value

## Tools

Python · pandas · mlxtend (Apriori, TransactionEncoder, association_rules) · matplotlib · seaborn
