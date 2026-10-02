# Country Clustering: Which Countries Need Aid the Most?

Unsupervised learning project that groups 167 countries by socio-economic and health indicators to decide where a humanitarian NGO should direct its funding first.

## Problem

HELP International is a humanitarian NGO that has raised roughly **$10 million**. The CEO needs to decide which countries are in the most urgent need of aid. This project clusters countries by their indicators and identifies the group that should be funded first.

## Dataset

`29-country_data.csv` contains one row per country with nine indicators: `child_mort`, `exports`, `health`, `imports`, `income`, `inflation`, `life_expec`, `total_fer` and `gdpp`.

Source: Kaggle, *Unsupervised Learning on Country Data*. Please check the dataset page for its license and terms of use.

## Approach

1. **Exploratory data analysis:** distributions, correlations and outliers. Most features are right-skewed and strongly correlated, and the outliers are real countries, so they are transformed rather than removed.
2. **Preprocessing:** Yeo-Johnson power transform, which handles the negative `inflation` values and standardizes each feature.
3. **PCA:** 4 components keep about 92% of the variance.
4. **Clustering:** K-Means, DBSCAN, HDBSCAN and Agglomerative (Ward) clustering, compared with silhouette, Davies-Bouldin and Calinski-Harabasz scores.
5. **Interpretation:** cluster profiles, a world map and a ranked list of priority countries.

## Results

I chose **K-Means with k = 3**. Silhouette is highest at k = 2, but two clusters only split the world into "needs aid" and "does not", which is too coarse for prioritising funding. With k = 3 the groups are balanced and easy to interpret:

| Cluster | Countries | Profile |
|---|---|---|
| Budget Needed | 51 | Very high child mortality (about 87 per 1,000), low income and GDP per capita, low life expectancy, high fertility |
| Medium Need | 65 | Middle values on all indicators |
| No Budget Needed | 51 | Low child mortality, high income and life expectancy |

Density-based methods (DBSCAN, HDBSCAN) did not fit this data well, because countries lie on a continuous development spectrum without clear low-density gaps.

The countries to prioritise first, ranked by child mortality and GDP per capita within the *Budget Needed* cluster, include Haiti, Sierra Leone, Chad, the Central African Republic and Mali. The full top-10 list is in the notebook.

## Limitations

- Silhouette scores are modest (about 0.27 for k = 3), so cluster boundaries are fuzzy.
- The analysis only uses the nine indicators in the dataset. Political, conflict and infrastructure data are not included.

## How to run

```bash
git clone https://github.com/haticeikrakalkan/country-clustering-aid-prioritization.git
cd country-clustering-aid-prioritization
pip install -r requirements.txt
jupyter notebook country_clustering.ipynb
```

## Files

- `country_clustering.ipynb`: the full analysis
- `29-country_data.csv`: the dataset
- `requirements.txt`: Python dependencies
