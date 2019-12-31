# ShelfLife

An easy to use Rust application that automatically spins down and expires unused
OKD projects.

This is an application I worked on during the CSH summer hackathon. It's used
to increase the ease of management of old OKD projects on CSH's Openshift
cluster. We don't have a good way of managing these applications, so here it is!

Currently, this app just queries the Openshift API for a particular project's
info and then queries a mongodb server for data, but it's got a few nifty
features planned:

* Run once daily via a cronjob
* Track all OKD namespaces, their admins, and when they were last deployed
  (or one of a few other timestamps, if those don't exist)
  * Store metrics on Openshift project lifespans
* Notify admins of those namespaces when their namespace hits a certain age
  (according to the build timestamp)
* Spin down, back up, and delete old projects to save resources

It will also have an interactive mode for setup, configuration, and monitoring.

## Build Dependencies

### Ubuntu (18.04 recommended)

- `build-essential`
- `libssl-dev`
- `pkg-config`

## Usage

To use this app, you'll need a few things:

* An Openshift cluster that can be accessed via API calls
* An admin™ account for ShelfLife to view and manage namespaces
* MongoDB installed and running
* A .env file to store Openshift cluster information, DB information, and a few
  other miscellaneous things. Get started by copying the provided `.env.sample`
  file to `.env` and then fill in the appropriate values.

## CLI Arguments
```
USAGE:
    shelflife [FLAGS] [OPTIONS]

FLAGS:
    -a, --all          Queries all available namespaces and adds/updates any that are missing/outdated to the database.
    -c, --cull         Checks greylist for projects that need attention. Takes appropriate course of action.
    -h, --help         Prints help information
    -l, --list         Print namespaces currently tracked in the database.
    -V, --version      Prints version information
    -w, --whitelist    Enables whitelist mode for that command, performing operations on the whitelist instead of the
                       greylist.

OPTIONS:
    -d, --delete <NAMESPACE>     Removes a namespace from the database.
    -k, --known <NAMESPACE>      Query API and ShelfLife Database for a known namespace. If it is missing from the
                                 database, the user is is asked if they want to add it.
    -p, --project <NAMESPACE>    Query API for project info about a namespace.
```
