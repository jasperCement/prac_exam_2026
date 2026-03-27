To get new course files added to your repository later, you will need to add the original repository (the one you forked) as a 'remote' [see here for help](https://stackoverflow.com/questions/3903817/pull-new-updates-from-original-github-repository-into-forked-github-repository),[and here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)  
To add updates/new files from the gen711-811 repo, copy and paste these lines into terminal on RON:
```
cd $HOME/gen711-811
git remote add upstream https://github.com/jthmiller/gen711-811.git
git fetch upstream
git merge upstream/master master
```
The first line is to get you back to your home directory just in case you switched. 
The second is to go looking for any changes that I might have made in my copy of 'gen711-811'
The third is to get any of those changes
The fourth is to merge my changes 'upstream/master' with your 'master'

Note, git merge is like "git pull" which is fetch + merge. Or, better, you can replay your local work on top of the fetched branch like a "git pull --rebase"
```
git rebase upstream/master
```



1. Ensure all your local changes are committed to your current branch.
- Save all your vscode additions, and commit them. 
2. Fetch the latest updates from the remote:
```git fetch origin```
3. Rebase your changes onto the updated remote branch
```git rebase origin/main```
4. Resolve any conflicts: If conflicts arise during the rebase, Git will pause the process and prompt you to resolve them. After editing the files to resolve the conflicts, use:
```
git add .
git rebase --continue
```
#### Resolving Conflicts
In both scenarios, if the same lines of code were changed in both your local work and the remote updates, Git will indicate a conflict. You must manually edit the conflicted files to choose which changes to keep. Your editor will show markers (like <<<<<<< and >>>>>>>) to help you identify the conflicting sections. 
After resolving the conflicts, add the file(s) and continue the operation as described above. 



Metadata - commas, spaces, tabs, special characters all bad
wget/curl get files from web
(command d) higlights every instance of a thing

> functions as a pipe to a location, will print to the file designated
> can also start a path, such as > ~/ to stick it in your home directory
>> will do it again (called appending), sticks two outputs together
wc -l is line count
piping greps into 'less' lets you scroll through the results
control R -v will show the opposite of your grepped less readout, displaying only those reads without the searched-for sequence
grep ^@ excludes quality lines with @'s, will find each sequence (every four lines)

for-loops 
for name in *.fastq
do
echo ${name}
done 

^ vairable names change based on your files
'for', 'do', and 'done' are the three important bits
braces for showing variable, quotes also work


1. Get practical exam into VS Code
cd ~/gen711
git clone <REPO_URL>
code <REPO_NAME>
2. Make 'analysis' directory from home
mkdir ~/analysis
3. Copy FASTQ files without changing directory
cp /tmp/gen711_2023/Sample1.fastq ~/analysis/
cp /tmp/gen711_2023/Sample2.fastq ~/analysis/

4. Change directory using absolute path
cd ~/home/users, etc/analysis

5. View top 4 lines of FASTQ
head -n 4 Sample1.fastq
8. Count reads with ≥15 Ns (no new file)
grep -E 'N{15,}' Sample1.fastq | wc -l
grep -E 'N{15,}' Sample2.fastq | wc -l

9. Create 'to_blast' and move FASTA files
mkdir to_blast
mv *.fasta to_blast/

10. Confirm files moved (without changing directory)
ls to_blast

11. Get 100th line of Sample1.fasta
head -n 100 to_blast/Sample1.fasta | tail -n 1

12. Run md5sum + save output
md5sum to_blast/Sample1.fasta
md5sum to_blast/Sample1.fasta > my_md5sums.txt

13. Append Sample2 md5sum
md5sum to_blast/Sample2.fasta >> my_md5sums.txt
14. Add your name
echo "MYNAME" >> my_md5sums.txt
Replace MYNAME with your real name.

Extra Credit (FastQC)
fastqc Sample1.fastq
fastqc to_blast/Sample1.fasta
Answer:
FASTQ works  (contains quality scores)
FASTA may fail or give warnings  (no quality scores)
Why: FastQC is designed for FASTQ format, which includes base quality information.

pwd — Print current directory
ls — List files
ls -l  - detailed view
ls -a  - show hidden files
cd — Change directory
cd ..   go up one level
mkdir — Make directory
mkdir analysis
mkdir -p dir1/dir2   - create nested dirs
cp — Copy files
cp file.txt backup.txt
cp file.txt ~/analysis/
cp *.fastq analysis/
mv — Move or rename
mv file.txt newname.txt
mv *.fasta to_blast/
rm — Delete
rm file.txt
rm -r folder/        # delete directory
Viewing Files - 
head — First lines
head file.txt
head -n 10 file.txt
tail — Last lines
tail file.txt
tail -n 5 file.txt
less — Scrollable view
less file.txt
cat — Print entire file
cat file.txt
Searching & Pattern Matching - 
grep — Search text
grep "ACTG" file.txt
grep -i "actg" file.txt     # ignore case
grep -c "ACTG" file.txt     # count matches
grep -E 'N{15,}' file.txt   # regex (15+ Ns)
Counting & Summaries - 
wc — Word/line count
wc file.txt
wc -l file.txt     # count lines
 Pipes & Redirection - 
Pipe |
Send output of one command into another
cat file.txt | grep ACTG
Redirect output >
ls > output.txt
Append output >>
echo "hello" >> file.txt

 FASTQ / FASTA Processing - 
FASTQ Structure (4 lines per read)
Header (@)
Sequence
+
Quality
Extract FASTA from FASTQ
Using awk (most important)
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq
Explanation:
NR%4==1 → header lines
NR%4==2 → sequence lines
substr($0,2) removes @
Preview output with head
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq | head
 Advanced Text Processing (AWK)
Print specific columns
awk '{print $1}' file.txt
Conditional filtering
awk '$1 > 10' file.txt
Regular Expressions (Regex Essentials) - 
Pattern
Meaning
.
any character
*
0 or more
+
1 or more
{15,}
15 or more
^
start of line
$
end of line

Example:
grep -E 'N{15,}' file.fastq
Line Selection Tricks - 
Get specific line (e.g., 100th line)
head -n 100 file.txt | tail -n 1
Checksums & File Integrity
md5sum
md5sum file.txt
md5sum file.txt > sums.txt
md5sum file2.txt >> sums.txt
Wildcards 
*.fastq        # all fastq files
Sample*        # files starting with "Sample"
Command History Shortcuts
Previous command
↑  (up arrow)
Repeat last command
!!
Quality Control (FastQC)
fastqc file.fastq
fastqc file.fasta
FASTA may fail (no quality scores)
Absolute vs Relative Paths
Absolute
cd /home/user/analysis
Relative
cd analysis
 Permissions - 
chmod +x script.sh
Combining Commands -
Example pipeline
grep -E 'N{15,}' file.fastq | wc -l
(Find reads with ≥15 Ns and count them)

Examples - 
Copy without changing directory
cp /path/to/file ~/analysis/
Convert + save file
awk 'NR%4==1 {print ">" substr($0,2)} NR%4==2 {print}' file.fastq > file.fasta
Check files in another directory
ls other_directory/

Add name to file
echo "Your Name" >> file.txt
Common Mistakes to Avoid - 
Forgetting / in paths
Using > instead of >> (overwrites file!)
Running commands in wrong directory
Forgetting quotes in awk or grep
Mixing up FASTQ vs FASTA format
The core jawns - 
cd
ls
cp
mv
mkdir
head
tail
grep
wc -l
awk
|   >   >>
