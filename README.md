# CV Keyword Matching – FairScreen Baseline (Track M, M2.1)

Naive keyword-matching baseline for ranking CVs against job requirements.
Dataset: Kaggle resume_data.csv (Neuralframe AI). Not included in the repo; place it in `data/`.

Baseline: for each job, count how many of the job's keywords appear in each CV's skills,
then rank CVs from highest to lowest count.