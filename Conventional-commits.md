The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set 
of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.

The commit message should be structured as follows:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

For more information, see https://www.conventionalcommits.org/en/v1.0.0/


## Adaptation to our working repositories

The recommendation is that the sum of labels, scope and description should not exceed 72 characters 
(recommendation <=50). From 72 characters github will separate and will not be complete in the commit history.


### Template

```
label(#issue_number): short_description
```

### Labels
  
- `fix`: A bug fix.
- `feat`: A new feature.
- `docs`: Documentation only changes.
- `refactor`: A code change that neither fixes a bug nor adds a feature. 
- `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc).
- `merge`: Commits that merge a branch into another branch.
- `revert`: Commits that reverse any change made, such as a PR.
- `ci`: Changes to our CI configuration files and scripts (`github actions`).
- `build`: Changes that affect the build system or external dependencies (`requirements.txt`, `setup.py`).
- `!`: Specifies that this commit is a breaking change. It is placed just before the `:`.

> **breaking change**: Change that modifies a behavior that may affect the rest of the components and their use. Occurs most often in shared libraries of code used by multiple applications. 


### Examples

```
fix(#3340): fix a uncaught exception
```

```
feat(#3324)!: add new mode for sending events 
```