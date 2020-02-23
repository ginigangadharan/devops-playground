https://stackoverflow.com/questions/59624053/upgrade-ansible-tower-minor-upgrade/59639499#59639499

If you used the docker-compose installation method and pointed postgres_data_dir to a persistent directory on the host, upgrading AWX is straightforward. I deployed AWX 2.0.0 in 2018 and have upgraded it to every subsequent release (currently running 9.1.0) without issue. Below is my upgrade method which preserves all data including secrets between upgrades and does not rely on using the tower cli / awx cli tool.

AWX path assumptions:

Existing installation: /opt/awx

New release: /tmp/awx

AWX inventory file assumptions:

use_docker_compose=true
postgres_data_dir=/opt/postgres
docker_compose_dir=/var/lib/awx
Manual upgrade process:

Backup your AWX host before continuing! Consider backing up your postgres database as well.
Download the new release of AWX and unpack it to /tmp/awx
Ensure that the patch package is installed on the host.
Create a patch file containing the differences between the new and existing inventory files:
diff -u /tmp/awx/installer/inventory /opt/awx/installer/inventory > /tmp/awx_inv_patch

Patch the new inventory file with the differences:
patch /tmp/awx/installer/inventory < /tmp/awx_inv_patch

Verify that the files now match:
diff -s /tmp/awx/installer/inventory /opt/awx/installer/inventory

Copy the new release directory over the existing one:
cp -Rp /tmp/awx/* /opt/awx/

Edit /var/lib/awx/docker-compose.yml and change the version numbers after image: ansible/awx_web: and image: ansible/awx_task: to match the new version of AWX that you're upgrading to.
Stop the current AWX containers:
cd /var/lib/awx

docker-compose stop

Run the installer:
cd /opt/awx/inventory

ansible-playbook -i inventory install.yml

AWX starts the upgrade process, which usually completes within a couple minutes. I'll typically monitor the upgrade progress with docker logs -f awx_web until I see RESULT 2 / OKREADY appear.

If everything is working as intended, I shut the containers down, pull and then recreate them using docker-compose:
cd /var/lib/awx

docker-compose stop

docker-compose pull && docker-compose up --force-recreate -d

If everything is still working as intended, I delete /tmp/awx and /tmp/awx_inv_patch.