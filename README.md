# hubatch
CLI tools for GitHub repository/organisation managers

## Features

There's a multitude of useful tools to help organisation and repository managers better manage their organisation/repository.

## Set-up/First Time

1. Install python (and maybe virtualenv + setup)
1. Install required dependencies using `pip` (viz. `pip install -r requirements.txt`)
1. Create + configure a `config.cfg` file. Use `config.sample.cfg` as a starting point.
   1. `APIKey` can be obtained by generating a personal access token on GitHub. Ensure that the user has the required permissions on the organisation/repository before generating the key.

## Using the program

The program is controlled entirely through a command line interface (CLI).

Execute the `main.py` (entry point) script with the following format:

```bash
$ python main.py [group] [tool] [tool-parameters ...|-h]
```

### Tools Available

* `issues` -- Issue Management Tools
  * `blast` -- Create duplicated issues for a list of GitHub users
  * `copy` -- Duplicate issues from one GitHub repository to another
* `orgs` -- GitHub Organisation Management Tools
  * `mass-add` -- Invite a list of users to join a particular organisation

To find out more about the positional/named parameters for a particular tool, run the tool with the `-h` flag.

You can use the example files located in the `example` folder as a reference. The hierarchy of the directory in `sample` is based on the group and tool (i.e. `issues blast` will be in the `issues/blast` folder in `example`).

## Troubleshooting

All log entries for this program is stored in `log.log` on the root directory
(i.e. where `main.py` resides)

### Getting the team ID for `mass-add`

You need to call the API to get the integer team ID for `mass-add` using `-t` option.

Use the following command on a `bash` shell to get the integer ID:
`curl -H "Authorization: token <put your token here>" https://api.github.com/orgs/<org-name-here>/teams`

This returns a JSON string from which you can retrieve the team ID.

### Continuing script for `copy`

Look for the log message of the form: `ERROR:root:[306][#1055][<from repo> -> <to repo>] Unable to copy`
The first number in the bracket is the index (here 306).
You can restart the script to run from 306 using the `-s <idx number>` option.
**However, beware, the next run index starts from 0, so if the script fails again, you need to add the previous failed indices to start at the right position.  If not you end up creating duplicate issues!**


### Github Abuse Detection

You cannot run the script start to end at once, as Github blocks the user account creating the issues owing to abuse detection mecanism kicking in.
Based on our observation, we can post ~300 issues in one run.
Hence, you may want to batch-fy your processing!

## Legal Notice

GitHub is a trademark of GitHub Inc. All other trademarks and servicemarks are the property of their respective owners/holders.
