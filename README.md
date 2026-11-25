# CareLoop Clinics — Appointment No-Show Analysis
repository link: https://github.com/sarthakgupta1903-ux/Careloop-noshow-analysis
## Summary

- **Part 1 — Data cleaning.** Cleaned 990 appointments, including 218 no-shows (22.0%).
- **Part 2 — SQL analysis.** Bookings made 31 or more days ahead had a 60.5% no-show rate (26 of 43).
- **Part 3 — Risk model.** Logistic regression achieved a test ROC-AUC of 0.789 and flagged 49 of 198 test bookings at a threshold of 0.35.
- **Part 4 — Follow-up notes.** All 9 nonblank notes passed validation and matched the expected categories. Five of the 10 appointments had no reminder, including both appointments classified as forgot_or_no_reminder.

## Recommendation

Pilot evening-before confirmation calls for high-risk appointments. Start with flagged bookings that had no reminder or were booked far in advance. Across the cleaned data, 22.0% of appointments were missed, and the rate reached 60.5% among the 43 bookings made at least 31 days ahead. At a threshold of 0.35, the model flagged 49 of 198 test bookings and caught 57.1% of actual no-shows. Both notes classified as `forgot_or_no_reminder` came from bookings without a reminder, so the front desk can use the call to confirm the visit or offer an easy way to reschedule. Track how many calls staff can complete and whether the pilot reduces missed appointments.

## Model and threshold selection

Logistic regression had the highest mean five-fold training ROC-AUC (0.7737), ahead of the tuned random forest (0.7673). On out-of-fold training predictions, lowering the threshold from 0.50 to 0.35 raised F1 from 0.347 to 0.489 and recall from 24.9% to 46.7%. The final held-out test ROC-AUC was 0.789, with precision and recall both at 57.1%. The per-booking test scores are in `test_risk_scores.csv`.

## Limitations

1. The 10 notes illustrate possible reasons for missed appointments. A larger sample is needed to estimate how common those reasons are across clinics.
2. Medians and outlier limits were calculated before splitting the data, so test rows indirectly influenced cleaning.
3. The random split can place bookings for the same patient in both training and test sets.
