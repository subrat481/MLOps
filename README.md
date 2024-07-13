# Dummy Test

Session-2
=========
conda env list

conda create --name mlops python=3.10
conda activate mlops
## conda deactivate
conda env remove --name mlops

conda env export > mlops_env.yml


conda env create -f mlops_env.yml


First Model Deployment
-----------------------
-> conda activate mlops
-> create a directory C1
-> prepare train.py to rain the model
-> run 
	pip install scikit-learn
	python train.py
-> prepare app.py for prediction
-> run
	pip install flask
	python app.py

curl -X POST -H "Content-Type: application/json" -d '{"features": [5.1, 3.5, 1.4, 0.2]}' http://127.0.0.1:5000/predict


Session-3
==========
Install docker into your machine (docker convenience script)
or
Download and Install Docker Desktop


If docker installed please check it using -> docker ps (in terminal)

-> create another directory C2
-> prepare Dockerfile
-> prepare requirement.txt
-> copy app.py and knn_model.joblib files into C2 directory

Now buid and run the dokcer image under C2 directory
----------------------------------------------------
-> docker build -t iris .
-> docker run -p 5000:5000 iris

Run this command -> curl -X POST -H "Content-Type: application/json" -d '{"features": [5.1, 3.5, 1.4, 0.2]}' http://127.0.0.1:5000/predict

Error:
curl: (56) Recv failure: Connection reset by peer

--> The issue here is we used a different python version


-> conda create --name mlops2 python=3.8
-> conda activate mlops2
-> pip install -r requirements.txt
-> python train.py
-> rm train.py
-> docker build -t iris .
-> docker run -p 5000:5000 iris

Run this command -> curl -X POST -H "Content-Type: application/json" -d '{"features": [5.1, 3.5, 1.4, 0.2]}' http://127.0.0.1:5000/predict

--> Still same issue occurred. Add (host='0.0.0.0') under app.py
--> app.run(debug=True, host='0.0.0.0')
--> whenever you use docker, explicitly you have to mention host. Normally it's not required.

Session-4
==========
Code Versioning
---------------
git clone https://github.com/subrat481/MLOps.git

(base) subratkumar@skumar-48MD6M MLOps % touch README.md
(base) subratkumar@skumar-48MD6M MLOps % git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
(base) subratkumar@skumar-48MD6M MLOps % git add README.md 
(base) subratkumar@skumar-48MD6M MLOps % git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md

(base) subratkumar@skumar-48MD6M MLOps % git config --global user.email "subrat.dataengineer@gmail.com"
(base) subratkumar@skumar-48MD6M MLOps % git commit -m "Adding README.md"
[master (root-commit) fcc37ba] Adding README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
(base) subratkumar@skumar-48MD6M MLOps % git branch -M main
(base) subratkumar@skumar-48MD6M MLOps % git push origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 228 bytes | 228.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/subrat481/MLOps.git
 * [new branch]      main -> main
(base) subratkumar@skumar-48MD6M MLOps % 


Again modify something and do below----->>>>

(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
(base) subratkumar@skumar-48MD6M MLOps % git add README.md                                             
(base) subratkumar@skumar-48MD6M MLOps % git commit -m "Updating README.md"
[main a81220d] Updating README.md
 1 file changed, 1 insertion(+)
(base) subratkumar@skumar-48MD6M MLOps % git push origin main              
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 271 bytes | 271.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/subrat481/MLOps.git
   fcc37ba..a81220d  main -> main


Data Versioning
---------------
Install dvc ---> pip install dvc


Go to folder /Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/MLOps 
(base) subratkumar@skumar-48MD6M MLOps % dvc init
Initialized DVC repository.

You can now commit the changes to git.

+---------------------------------------------------------------------+
|                                                                     |
|        DVC has enabled anonymous aggregate usage analytics.         |
|     Read the analytics documentation (and how to opt-out) here:     |
|             <https://dvc.org/doc/user-guide/analytics>              |
|                                                                     |
+---------------------------------------------------------------------+

What's next?
------------
- Check out the documentation: <https://dvc.org/doc>
- Get help and share ideas: <https://dvc.org/chat>
- Star us on GitHub: <https://github.com/iterative/dvc>
(base) subratkumar@skumar-48MD6M MLOps % 


It will create .dvc folder along with necessary dirs inside the MLOps folder.

--> Create another folder inside MLOps folder
--> Keep some data file into this folder and check git status

Check the Git Status--->
(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .dvcignore

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        data/

--> Create the default remote for dvc for remote storage (some server or any location s3 etc or any basket or any db also)
dvc remote add -d myremote '/path_of_default_remote_for_dvc'
e.g: dvc remode add -d myremote '/Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/remote_storage'

--> To do this, create another folder remote_storage outside the MLOps directory or provide some directory location to make that dvc remote storage
eg: dvc remode add -d myremote '/Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/remote_storage'

Now Check the Git Status--->

(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .dvcignore

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .dvc/config

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        data/

-->Here you can clearly observe that above path has been added as remote path in config file under .dvc folder.

--> Now you can add your data file to dvc using:
dvc add '/Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/MLOps/data/cancer_drugs_public_V13_0.csv'
(base) subratkumar@skumar-48MD6M MLOps % dvc add '/Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/MLOps/data/cancer_drugs_public_V13_0.csv'
100% Adding...|████████████████████████████████████████████████████████████████████████████████████████████████████████████████|1/1 [00:00,  5.36file/s]
                                                                                                                                                        
To track the changes with git, run:

        git add data/.gitignore data/cancer_drugs_public_V13_0.csv.dvc

To enable auto staging, run:

        dvc config core.autostage true

--> The moment you add cancer_drugs_public_V13_0.csv, there will be one new file created under data folder names as cancer_drugs_public_V13_0.csv.dvc. Its a metadata file which you can open and see.

--> And finally you do dvc push
(base) subratkumar@skumar-48MD6M MLOps % dvc push
Collecting                                                                                                          |1.00 [00:00,  226entry/s]
Pushing
1 file pushed   


--> You can see there is a file created newly names with some hash value under remote_storage folder.
--> Now you can delete cancer_drugs_public_V13_0.csv file from data folder. There is still cancer_drugs_public_V13_0.csv.dvc file will be present.
--> And if you want to pull the actual data, you can do that as below:
(base) subratkumar@skumar-48MD6M MLOps % dvc pull
Collecting                                                                                                          |1.00 [00:00,  250entry/s]
Fetching
Building workspace index                                                                                            |1.00 [00:00,  172entry/s]
Comparing indexes                                                                                                   |3.00 [00:00,  858entry/s]
Applying changes                                                                                                    |1.00 [00:00,   149file/s]
A       data/cancer_drugs_public_V13_0.csv
1 file added


--> Check the git status, your actual cancer_drugs_public_V13_0.csv file will not be going to git push as its a part of .gitignore

(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .dvcignore

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .dvc/config

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        data/

(base) subratkumar@skumar-48MD6M MLOps % git add .
(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dvc/.gitignore
        new file:   .dvc/config
        new file:   .dvcignore
        new file:   data/.gitignore
        new file:   data/cancer_drugs_public_V13_0.csv.dvc

--> Pushing the changes to remote

(base) subratkumar@skumar-48MD6M MLOps % git commit -m "dvc example"
[main 368c263] dvc example
 5 files changed, 16 insertions(+)
 create mode 100644 .dvc/.gitignore
 create mode 100644 .dvc/config
 create mode 100644 .dvcignore
 create mode 100644 data/.gitignore
 create mode 100644 data/cancer_drugs_public_V13_0.csv.dvc

(base) subratkumar@skumar-48MD6M MLOps % git push origin main
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 979 bytes | 195.00 KiB/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To https://github.com/subrat481/MLOps.git
   a81220d..368c263  main -> main


--> You can go to git repository and check the actual cancer_drugs_public_V13_0.csv file data is not pushed. But still we have dvc to pull that data back from remote_storage.


---> Now any changes that happenes to our data that can be clearly track by dvc. Do some change into the data file and hit below command
dvc status

(base) subratkumar@skumar-48MD6M MLOps % dvc status
data/cancer_drugs_public_V13_0.csv.dvc:                                                                                                       
        changed outs:
                modified:           data/cancer_drugs_public_V13_0.csv

--> Now repeate the same process

(base) subratkumar@skumar-48MD6M MLOps % dvc add '/Users/subratkumar/Downloads/backup/BITS-PILANI-MTECH/AIML_SEM-3/MLOps (S2-23_AIMLCZG523)/Code/C3/MLOps/data/cancer_drugs_public_V13_0.csv'
100% Adding...|██████████████████████████████████████████████████████████████████████████████████████████████████████|1/1 [00:00, 18.70file/s]
                                                                                                                                              
To track the changes with git, run:

        git add data/cancer_drugs_public_V13_0.csv.dvc

To enable auto staging, run:

        dvc config core.autostage true

(base) subratkumar@skumar-48MD6M MLOps % dvc status
Data and pipelines are up to date.                

(base) subratkumar@skumar-48MD6M MLOps % dvc push 
Collecting                                                                                                          |1.00 [00:00,  242entry/s]
Pushing
1 file pushed

(base) subratkumar@skumar-48MD6M MLOps % dvc status
Data and pipelines are up to date.

--> Git push

(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   data/cancer_drugs_public_V13_0.csv.dvc

no changes added to commit (use "git add" and/or "git commit -a")

(base) subratkumar@skumar-48MD6M MLOps % git add .
(base) subratkumar@skumar-48MD6M MLOps % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   data/cancer_drugs_public_V13_0.csv.dvc

(base) subratkumar@skumar-48MD6M MLOps % git commit -m "data changes"
[main ee6b065] data changes
 1 file changed, 2 insertions(+), 2 deletions(-)

(base) subratkumar@skumar-48MD6M MLOps % git push origin main
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 415 bytes | 415.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/subrat481/MLOps.git
   368c263..ee6b065  main -> main


---> If you want you can track the git log also by using git log command
(base) subratkumar@skumar-48MD6M MLOps % git log
commit 8460f41aea96916b94545c0aa6fd54d6f679e69c (HEAD -> main, origin/main, origin/HEAD)
Author: Subrat Kumar <subrat.dataengineer@gmail.com>
Date:   Sat Jul 13 18:51:31 2024 +0530

    data reverted

commit ee6b0652abe170f3c925ce6a98178dd4adaf32cf
Author: Subrat Kumar <subrat.dataengineer@gmail.com>
Date:   Sat Jul 13 18:45:41 2024 +0530

    data changes

commit 368c26326e5b085705a948fcd5e9debfb082e7a6
Author: Subrat Kumar <subrat.dataengineer@gmail.com>
Date:   Sat Jul 13 18:32:33 2024 +0530

    dvc example

commit a81220d854d0384526a5adaf9f9c50880454d5f4
Author: Subrat Kumar <subrat.dataengineer@gmail.com>
Date:   Fri Jul 5 23:42:21 2024 +0530

    Updating README.md

commit fcc37ba088f26bde1d27046495730370b7ceffc1
Author: Subrat Kumar <subrat.dataengineer@gmail.com>
Date:   Fri Jul 5 23:30:56 2024 +0530

    Adding README.md
