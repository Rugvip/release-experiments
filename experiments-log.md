# Experiments

# Grandiose Idea

Allow a mix of 0.x.x (not considered next) with 1.x.x-next.x

## Experiment 1

- make a prerelease and then full release of baz

https://github.com/changesets/changesets/blob/main/docs/prereleases.md

- `yarn changeset pre enter next`

  ```json
  {
    "mode": "pre",
    "tag": "next",
    "initialVersions": {
      "bar": "0.1.0",
      "baz": "0.1.0",
      "foo": "0.1.0"
    },
    "changesets": []
  }
  ```

- `yarn changeset`

  create changeset with minor bump of baz called `tall-peaches-hear.md`

- `yarn changeset version`

  baz bumped to 0.2.0-next.0

  ```json
  {
    "mode": "pre",
    "tag": "next",
    "initialVersions": {
      "bar": "0.1.0",
      "baz": "0.1.0",
      "foo": "0.1.0"
    },
    "changesets": ["tall-peaches-hear"]
  }
  ```

- `yarn changeset pre exit`

  ```json
  {
    "mode": "exit",
    "tag": "next",
    "initialVersions": {
      "bar": "0.1.0",
      "baz": "0.1.0",
      "foo": "0.1.0"
    },
    "changesets": ["tall-peaches-hear"]
  }
  ```

- `yarn changeset version`

  - baz bumped to 0.2.0
  - pre.json removed
  - changesets removed

## Experiment 2

- make two pre-releases of baz, 0.2.0-next.0 and 0.2.0-next.1

Works as expected with each `yarn changeset version` creating a new version.

Noteworthy is that the bump may shift, for example `0.1.1-next.0` to
`0.2.0-next.1`.

## Experiment 3

- pre-release of baz. Make a regular release of foo, during baz pre-release.

WE HAVE A HUGE PRAHBABLEM:

How could we have new versions of 0.x stuff, that depends on features of that
are only available in pre-releases of 1.x stuff?

Well let's just have 0.x packages depend on the pre-releases then

EDIT: NOPE, we don't allow 0.x packages to depend things from 1.x pre-releases

Isolated non-prerelease:

- Delete all of the changesets listed in pre.json and pre.json itself
- Run `yarn changeset version`
- Non-prerelease was made!

## Experiment 4

What happens to the dependent's version when you bump a pre-release dependency?

-> Nothing good, created https://github.com/changesets/changesets/issues/724
