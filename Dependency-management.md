## Go dependencies

We are using [`dep`](https://github.com/golang/dep) as our Go dependency management tool. It is the official prototype, that most probably will become the official tool in the near future.

In order to speed up the whole development process, we have decided to keep dependency files inside our repository. `.gitignore` file helps us to ignore redundant files like tests etc.

### Updating Go dependencies

1. Have the Dashboard checked out in `${GOPATH}/src/github.com/kubernetes/dashboard`.
2. Have [`dep`](https://github.com/golang/dep) installed.
3. Create new branch.
4. Update dependency versions in `Gopkg.toml`.
5. Run `dep ensure -update && dep prune` to update `vendor` contents and keep only used files.
6. Build and run the unit tests. If they are broken most likely some of our vendor packages have
changed API and it needs to be fixed.
7. Commit the changes in `vendor` directory and in our sources separately for easier review. It is important to not remove `.gitignore` file at this point.
8. Send a pull request.