Workflow description:

`reusable-ci.yml`
**This workflow needs to be called from another workflow.**

1. Creates a PostgresDB (from a [Docker image](https://hub.docker.com/_/postgres)).
2. Installs Ruby based on the version specified in `.ruby-version` file.
3. Installs and caches the dependencies using `Gemfile.lock` file.
4. Precompiles assets.
5. Sets up DB running `db:create` and `db:migrate`.
6. Runs [RSpec](https://github.com/rspec) test suite.

`reusable-cd.yml`
**This workflow needs to be called from another workflow.**

1. Uses [heroku-deploy](https://github.com/AkhileshNS/heroku-deploy) action to perform the deployment based on the received `secrets` and `inputs`.
2. Runs DB migrations after the deployment.


`development.yml`
- Calls `reusable-ci.yml` to build and test the development branch when pushing or creating a PR to that branch.
- Won't perform deployments.
- Won't be triggered for `main` and `staging` branches.

`staging.yml`
- Calls `reusable-ci.yml` to build and test when pushing to `staging` branch.
- If the CI succeeds, calls `reusable-cd.yml` to deploy to staging environment.

`production.yml`
- Calls `reusable-ci.yml` to build and test when pushing to `main` branch.
- If the CI succeeds, calls `reusable-cd.yml` to **deploy to production environment**.



**Remember to configure your secrets from Settings > Secrets > Actions**
