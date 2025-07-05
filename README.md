

<p align="center">
  AuditFlow is a tool designed for identifying vulnerabilities in open source codebases at scale. It can gather and filter on key repository metrics such as popularity and project size, enabling targeted vulnerability research. It automatically detects potential vulnerabilities using semgrep and provides a streamlined HTML report, allowing researchers to quickly drill down to the affected portion of the codebase.
  <br>


<p align="center">
  <a href="#examples">Examples</a> •
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a>
</p>

<video src="https://github.com/user-attachments/assets/bda95efd-07ee-46f3-82f0-b37e229ac352" autoplay muted loop playsinline style="max-width: 100%;"></video>


# Examples

Oneliner to scrape every HackerOne open source target and run semgrep on it
```sh
bbscope h1 -b -u '<HACKERONE_USERNAME>' -t '<HACKERONE_TOKEN>' -o tdu | grep -E 'https?://github.com/[A-Za-z0-9-]{1,}/[A-Za-z0-9-]{1,}' -o  | sastsweep -threads 10 -desc -stars -files
```

Scrape flask applications from github search using [github-search.py](github-search.py) and filter on repositories with 500-3000 stars.  Display the number of stars, the repository description, and number of files.
```sh
python3 github-search.py --token '<GITHUB TOKEN>' --query '"import Flask" AND ".route("' | sastsweep -stars -desc -files -filter-stars 500-3000
```

Scan a single repository, display the number of stars, number of security issues, and date of the last commit
```sh
sastsweep -repo https://github.com/chebuya/SASTsweep -stars -security-issues -last-commit
```

Scan a list of targets, display the star count, language composition, number of forks and number of contributors.  Filter on repositories with a last commit date after 2024/01/01, less than 5000 stars, and 0 security issues
```sh
sastsweep -repos targets.txt -stars -lang -forks -contributors -filter-last-commit 2024/01/01- -filter-stars -5000 -filter-security-issues 0
```


Run with p/python config
```
sastsweep -config-path sastsweep.conf ...
```

sastsweep.conf
```
FLAGS=--config p/python
```

# Installation
Linux is currently the only supported and tested platform <br>
`sastsweep` requires go >= 1.23 to install successfully. Run the following command to install `sastsweep`
```sh
go install github.com/chebuya/sastsweep/cmd/sastsweep@latest
```

# Usage

```sh
sastsweep -h
```

This will display help for the tool. Here are all the switches it supports.

```console
Usage of ./sastsweep:
  -branch
    	Display the default branch of a repository
  -commits
    	Display the number of commits to the repository
  -config-path string
    	Path to semgrep.conf file
  -contributors
    	Display the number of contributors in a repository
  -debug
    	Enable debug messages
  -desc
    	Display repo description
  -files
    	Display number of files in repo
  -filter-commits string
    	Filter the number of commits to the repository (500-700, -300, 500-, 3000)
  -filter-contributors string
    	Filter the number of contributors in a repository (500-700, -300, 500-, 3000)
  -filter-files string
    	Filter number of files in repo (500-700, -300, 500-, 3000)
  -filter-first-commit string
    	Filter the date of the first commit to the repository (yyyy/mm/dd-yyyy/mm/dd, -yyyy/mm/dd, yyyy/mm/dd-, yyyy/mm/dd)
  -filter-forks string
    	Filter the number of forks of repository (500-700, -300, 500-, 3000)
  -filter-issues string
    	Filter the number of issues in a repository (500-700, -300, 500-, 3000)
  -filter-last-commit string
    	Filter the date of the last commit to the repository (yyyy/mm/dd-yyyy/mm/dd, -yyyy/mm/dd, yyyy/mm/dd-, yyyy/mm/dd)
  -filter-last-release string
    	Filter the date of the latest release (yyyy/mm/dd-yyyy/mm/dd, -yyyy/mm/dd, yyyy/mm/dd-, yyyy/mm/dd)
  -filter-pull-requests string
    	Filter the number of pull requests in a repository (500-700, -300, 500-, 3000)
  -filter-security-issues string
    	Filter the number of security issues in the repository (500-700, -300, 500-, 3000)
  -filter-stars string
    	Filter repos stars in output (500-700, -300, 500-, 3000)
  -filter-watchers string
    	Filter the number of watchers in a repository (500-700, -300, 500-, 3000)
  -fireprox string
    	Use fireprox for reasons... relates to rate limiting on a certain platform (ex: https://abcdefghi.execute-api.us-east-1.amazonaws.com/fireprox/)
  -first-commit
    	Display the date of the first commit to the repository
  -forks
    	Display the number of forks of repository
  -full-desc
    	Display the full repo description
  -github1s
    	Generate links for the web-based vscode browser at github1s.com rather than github.com
  -issues
    	Display the number of issues in a repository
  -lang
    	Display GitHub repo language
  -last-commit
    	Display the date of the last commit to the repository
  -last-release
    	Display the date of the latest release
  -no-emoji
    	Disable this if you are a boring person (or use a weird terminal)
  -no-semgrep
    	Do not perform a semgrep scan on the repos
  -out-dir string
    	Directory to clone repositories to
  -pull-requests
    	Display the number of pull requests in a repository
  -raw-links
    	Print raw links for semgrep report rather than hyperlink with name, good if you want to save output
  -repo string
    	GitHub repository to scan
  -repo-link
    	Display the link associated with the repository
  -repos string
    	File of GitHub repositories to scan
  -save-repo
    	Save the cloned repository
  -security-issues
    	Display the number of security issues in the repository
  -semgrep-path string
    	Custom path to the semgrep binary
  -stars
    	Display repos stars in output
  -threads int
    	Number of threads to start (default 3)
  -topics
    	Display GitHub repo topics
  -watchers
    	Display the number of watchers in a repository
```


