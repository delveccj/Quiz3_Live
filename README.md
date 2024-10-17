# Quiz3 Live

# CSC395 Quiz 3!

## Instructions

### Prep Steps
You need to ensure you are logged into GitHub via gh.  To do so, issue the following command:



### 1. Create a GitHub Repository
1. Log in to your GitHub account.
2. Create a new **private repository** named `CSC395_Quiz3`.
3. Add a README and a licesne to the repositry.
4. Invite me (`delveccj`) as a collaborator to your repository.

### 2. Clone Your Repository Locally
1. Open your terminal and clone the repository.
2. Navigate to the cloned directory:
   ```bash
   cd CSC395_Quiz3
   ```
   
You may run into an error as follows (if you have no error and you cloned without issue, move to Step 3):

```bash
MC-28907:Quiz3 delveccj$ git clone https://github.com/delveccj/Quiz3.git
Cloning into 'Quiz3'...
remote: Repository not found.
fatal: repository 'https://github.com/delveccj/Quiz3.git/' not found
```
If so, you will need to login to GitHub once again (the credentials provided by the browser may have expired.)  Here are the steps:

```bash
MC-28907:Quiz3 delveccj$ gh auth login
? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations on this host? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Login with a web browser

! First copy your one-time code: CB69-1689
Press Enter to open github.com in your browser... 
✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as delveccj
! You were already logged in to this account
```

### 3. Add Python Script for Basic Text Analytics
1. Download or copy the following Python script (`date_extractor.py`), which extracts all the dates from a document:
   ```python
   from dateutil.parser import parse
   import re
   
   def extract_dates_from_text(text):
       # Use a regex to find potential date-like patterns
       date_candidates = re.findall(r'\b[\w\s\-,]+\b', text)
       dates = []
   
       for candidate in date_candidates:
           try:
               # Attempt to parse the candidate as a date
               parsed_date = parse(candidate, fuzzy=True)
               dates.append(parsed_date)
           except ValueError:
               # Skip if the candidate isn't a valid date
               pass
   
       return dates
   
   if __name__ == "__main__":
       with open("sample.txt", "r") as f:
           content = f.read()
           dates = extract_dates_from_text(content)
           print("Extracted Dates:")
           for date in dates:
               print(date.strftime("%Y-%m-%d"))
   ```
2. Save the script in your local repository directory as `date_extractor.py`.

### 4. Create a Sample Text File
1. Create a sample HTML file called `sample.txt` in the same directory:
   ```text
   The project started on 2023-09-16.
   The deadline is October 20, 2023.
   We met again on 15th October 2023.
   ```

### 5. Create a Unit Test for the Script
1. Create a Python test script `test_date_extractor.py`:
   ```python
   import unittest
   from date_extractor import extract_dates_from_text
   
   class TestDateExtractor(unittest.TestCase):
       def test_extract_dates(self):
           with open("sample.txt", "r") as f:
               text = f.read()
           
           result = extract_dates_from_text(text)
   
           # Expected dates in the format (Year, Month, Day)
           expected_dates = [
               "2023-09-15",
               "2024-10-20",
               "2023-10-15"
           ]
           extracted_dates = [date.strftime("%Y-%m-%d") for date in result]
   
           self.assertEqual(extracted_dates, expected_dates)
   
   if __name__ == '__main__':
       unittest.main()
   ```

2. Ensure that the tests pass.  They will not at outset!

### 6. Create a `requirements.txt` File
1. Install **python-dateutil**:
   ```bash
   pip install python-dateutil
   ```
2. Create a `requirements.txt` file that will install ```python-dateutil```

### 7. Fix the Failing Unit Test

Run the unit tests as follows:

```bash
python3.10 -m unittest test_date_extractor.py
```
The test will fail.  You will need to fix the test's input file to make it work!

### 8. Add a GitHub Actions Workflow
1. Create a directory `.github/workflows` in the root of your repository:
   ```bash
   mkdir -p .github/workflows
   ```
2. Create a file called `workflow.yml` inside `.github/workflows` with the following content:
   ```yaml
   name: Date Extractor Workflow

   on:
     push:
       branches:
         - main  # Trigger workflow when code is pushed to the main branch

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
       # Step 1: Checkout the repository
       - name: Checkout code
         uses: actions/checkout@v2

       # Step 2: Set up Python 3.x
       - name: Set up Python
         uses: actions/setup-python@v2
         with:
           python-version: '3.x'

       # Step 3: Install dependencies
       - name: Install dependencies
         run: |
           python -m pip install --upgrade pip
           pip install -r requirements.txt

       # Step 4: Run the unit tests
       - name: Run unit tests
         run: |
           python -m unittest discover

       # Step 5: Check for specific commit comment
       - name: Check for specific commit comment
         run: |
           git log -1 --pretty=%B | grep "Live quiz complete"
         continue-on-error: false
   ```

### 9. Add, Commit, and Push Your Changes

1. Add all the files to Git **except** ```__pycache__```  You must not add that directory.
2. Commit your changes with this exact message "Live quiz complete"  Note that the workflow will test for this message so you need the correct case!
3. Push your changes to GitHub

---

## Submission Instructions
1. Ensure the repository contains:
   - `date_extractor.py`
   - `test_date_extractor.py`
   - `sample.txt`
   - `requirements.txt`
   - GitHub Actions workflow in `.github/workflows/workflow.yml`
2. Ensure all tests pass in the GitHub Actions workflow.
3. Make sure your last commit message includes the text `"Live quiz complete"`.
4. Once done, ensure all changes are pushed to the repository, and notify me (via GitHub or email) that your quiz is complete.

---

Good luck! 🎉
```
