# ERA V5 Lab

The working repository of the ERA V5 training lab (The School of AI).

## Week 1 (Sat 24 Oct to Sat 31 Oct 2026): data diagnosis

Every student starts with the data diagnosis task.

1. Find your pack ID and your download link on the lab plan sheet. The link is valid for 7 days.
2. Install: `pip install pyarrow`.
3. Read your pack: `python diagnosis/tools/view_pack.py <pack_id>.parquet`.
4. Label each document. Follow `diagnosis/DISEASES.md`.
5. Write the medicine sheet. Run the harm test.
6. Submit with a pull request. Follow `diagnosis/submissions/README.md`.

Due dates (IST):

- **Tue 27 Oct, 23:59:** the first 150 labelled rows.
- **Sat 31 Oct, 23:59:** all rows, and the medicine sheet with the harm tests.

The 3-day rule applies. Data tasks get 7 days.

## Team folders

`teams/<NN>-<name>/` holds the work of each team. The team lead merges pull requests.
