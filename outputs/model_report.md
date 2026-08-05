# FlyRank Refresh Opportunity Model Report

This report is generated from the bundled anonymized starter dataset (`data/raw/content_refresh_anonymized.csv`).
The model ranks existing content for refresh review. It does not use titles, URLs, client names, domains, or keywords.

## Data

- Rows scored: 30,000
- Declining-label rows: 16,262
- Declining-label rate: 0.542
- Split strategy used for validation: client_holdout
- Target: `is_declining_label`

## Model Comparison

Best model: `gradient_boosting` selected by `precision_at_50`.

| Model | ROC AUC | Avg precision | Precision@50 | Recall | F1 |
|---|---:|---:|---:|---:|---:|
| decision_tree | 0.735 | 0.572 | 0.500 | 0.713 | 0.633 |
| gradient_boosting | 0.777 | 0.688 | 0.940 | 0.773 | 0.649 |
| logistic_regression | 0.707 | 0.537 | 0.480 | 0.564 | 0.571 |
| random_forest | 0.744 | 0.624 | 0.840 | 0.718 | 0.628 |
| baseline_rules | 0.627 | 0.468 | 0.240 | - | - |

## Final Queue

- High-confidence items: 3,207
- Medium-confidence items: 11,793
- Low-confidence items: 15,000
- `monitor` items: 12,250
- `refresh` items: 8,813
- `refresh_and_review_ctr` items: 6,842
- `refresh_and_review_engagement` items: 2,013
- `expand_and_refresh` items: 82

## Top Features

- `days_with_impressions`: 0.1177
- `avg_position`: 0.0583
- `log_impressions_90d`: 0.0434
- `content_age_days`: 0.0432
- `scroll_rate`: 0.0200
- `ctr`: 0.0171
- `word_count`: 0.0168
- `update_age_ratio`: 0.0152
- `char_count`: 0.0139
- `days_with_sessions`: 0.0087

## Top 10 Queue Preview

| Rank | Score | Model probability | Action | Reasons | Impressions | Sessions | Trend |
|---:|---:|---:|---|---|---:|---:|---|
| 1 | 96.5 | 0.981 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 26287 | 685 | down |
| 2 | 95.8 | 0.969 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 12029 | 119 | down |
| 3 | 95.1 | 0.939 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 28000 | 345 | down |
| 4 | 94.7 | 0.960 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 9594 | 89 | down |
| 5 | 94.5 | 0.988 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 7485 | 192 | down |
| 6 | 94.4 | 0.977 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 40305 | 408 | down |
| 7 | 93.4 | 0.955 | refresh_and_review_ctr | declining_with_demand, low_ctr_visible_page, model_decline_risk, visible_model_opportunity, ctr_review_candidate | 8012 | 17 | down |
| 8 | 92.8 | 0.965 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 14099 | 84 | down |
| 9 | 92.8 | 0.971 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 10744 | 34 | down |
| 10 | 92.8 | 0.984 | refresh_and_review_engagement | declining_with_demand, low_engagement_visible_page, model_decline_risk, visible_model_opportunity, engagement_review_candidate | 6510 | 52 | down |

## Generated Files

- `outputs/refresh_queue.csv`
- `outputs/model_results.json`
- `outputs/summary.json`
- `outputs/charts/action_mix.svg`
- `outputs/charts/confidence_mix.svg`
- `outputs/charts/top_reason_codes.svg`
- `outputs/charts/top_feature_importance.svg`
- `outputs/charts/trend_distribution.svg`

## Practical Use

Use the ranked queue as a reviewer aid, not as an automatic publishing decision.
The safest first production use is to inspect high-confidence rows, verify the page manually, and compare the recommendation against editorial context.
