# How to submit

## Where

Make one folder for your pack:

```
diagnosis/submissions/<pack_id>/<your-github-handle>/
    labels.csv     (from diagnosis/templates/labels_TEMPLATE.csv)
    medicine.md    (from diagnosis/templates/medicine_TEMPLATE.md)
```

Your `pack_id` is on the CAP plan sheet. Do not put the pack data (the Parquet file) in the repository.

## How

1. Fork https://github.com/theschoolofai/era-v5-lab to your GitHub account.
2. Clone your fork and make a branch:
   ```
   git clone https://github.com/<your-github-handle>/era-v5-lab.git
   cd era-v5-lab
   git checkout -b diag-<pack_id>-<your-github-handle>
   ```
3. Add your folder and the two files. Then commit and push:
   ```
   git add diagnosis/submissions/<pack_id>/<your-github-handle>/
   git commit -m "diagnosis: <pack_id> <your-github-handle>"
   git push -u origin diag-<pack_id>-<your-github-handle>
   ```
4. Open one pull request to `theschoolofai/era-v5-lab`, branch `main`.
5. **The first lines of the pull request description must be:**
   ```
   Name: <your name as on Axiom>
   Axiom email: <your Axiom email>
   Pack: <pack_id>
   ```
   We use these lines to match your pull request to your name.
6. To update your work, push new commits to the same branch. The same pull request updates. Do not open a second pull request.

## Due dates (IST)

- **Tue 27 Oct, 23:59:** at least 150 labelled rows in `labels.csv`.
- **Sat 31 Oct, 23:59:** all rows, and `medicine.md` with a harm test for each medicine.

We read the last version of your pull request at each due date.

## Rules

- **Never copy personal data or secrets.** For F1 and E3, give only the type and the position (for example
  `email at offset 64`). Never the value.
- Change only your own folder.
- Team work goes to `teams/<NN>-<name>/` in the same way. Your team lead reviews it.
