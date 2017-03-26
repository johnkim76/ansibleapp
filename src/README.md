# ansible playbook bundle tooling

> NOTE: Project is still a WIP and rapidly changing.

`ansibleapp` is a tool for helping Ansible Playbook Bundle (APB) authors create, build, and publish
their apbs to container registries. It enforces best practices and takes
care of the details so they should be easy to deploy.

## Install

**System installation**:

`sudo pip install -r requirements.txt && sudo python setup.py install`

For non-priviledged installs such as virtualenvs, drop sudo.

**Development**:

`pip install -e $SRC_DIR` will install in editable mode.

## Commands

### Prepare

`apb prepare` is a helper that will generate a `Dockerfile` with
your `apb.yml` file encoded as a base64 label. This allows the spec
to be accessible to clients via registry APIs without the necessity of downloading
full images. For example, the [Ansible Service Broker](https://www.github.com/fusor/ansible-service-broker)
will use this data to build an inventory of available apbs in a given
registry and expose them to an Openshift or Kubernetes cluster.

Prepare expects an `ansible/` directory at the targeted project location. This
can be created using `ansible-container {build, push, shipit}` as prerequisites.
See [Using ansible-container to create an apb](https://github.com/fusor/ansibleapp/blob/master/docs/developers.md#using-ansible-container-to-create-an-apb).

By default, the CWD is used, but it will accept a directory location via
`--project=$LOCATION` as well.

Prepare also expects a specfile, `apb.yml`, to exist at the project root.
[More on specfiles and expected contents.](https://github.com/fusor/ansibleapp/blob/master/docs/design.md).

Following `apb prepare`, you should have a Dockerfile and a correctly
ID'd specfile. Build and publish this container to your preferred registry.
