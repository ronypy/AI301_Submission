I added a missing, runnable example for anomaly detection on data streams to PySAD, an open source Python library. The pull request is open and waiting for the maintainer's review.

The library already had a detector called MCOD, which flags unusual data points by checking how many of the most recent points sit close to a new one. But none of its 59 examples showed anyone how to use it, so I wrote one, plus the matching documentation.

What I learned:
- Nothing was broken. I proved the gap with a simple test that found zero examples for MCOD before my change and one passing example after it.
- I tried four settings before choosing one. The accuracy score ranged from 0.73 to 0.88, so I picked the setting that explains the idea best and said so in the code.
- My own pre-submission checker caught a real mistake: my description did not follow the project's rule on disclosing AI help. I fixed it before anyone had to ask.

The project maintainer assigned me the issue and the automated checks passed, but it is not merged yet, so I'm not calling it done. Next I'll respond to review and suggest examples for the other detectors separately.

Here is the pull request:
https://github.com/selimfirat/pysad/pull/656

#OpenSource #Python #AnomalyDetection #FirstContribution
