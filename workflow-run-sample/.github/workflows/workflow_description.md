Workflow description:

`ci.yml`
This workflow will be triggered on pushing to any branch.

1. Creates a PostgresDB (from a [Docker image](https://hub.docker.com/_/postgres)).
2. Installs Ruby based on the version specified in `.ruby-version` file.
3. Installs and caches the dependencies using `Gemfile.lock` file.
4. Precompiles assets.
5. Sets up DB running `db:create` and `db:migrate`.
6. Runs [RSpec](https://github.com/rspec) test suite.

`production-deploy.yml`
- Is automatically triggered after `ci.yml` when:
  - Pushing to `main` branch.
  - CI completed successfully
- Uses [heroku-deploy](https://github.com/AkhileshNS/heroku-deploy) action to **deploy to production environment** based on the secrets in your repo.
- Runs DB migrations after the deployment.

`staging-deploy.yml`
- Is automatically triggered after `ci.yml` when:
  - Pushing to `staging` branch.
  - CI completed successfully
- Uses [heroku-deploy](https://github.com/AkhileshNS/heroku-deploy) action to deploy to staging environment based on the secrets in your repo.
- Runs DB migrations after the deployment.

**Remember to configure your secrets from Settings > Secrets > Actions**
