# Repo to set up all objects within existing AAP 
# works for usecase 1 and 2
# the Repo is flexible enough to deploy any usecase by only changing extra_vars 


##Needs to run form someone with SSH key to connect to the RHEL template used later...
## HandsOnLab@Deployment
## ansible@bastion21
## mschreie@mschreie
## mschreie@rhel9msi.example.com

# I'm working with execution environments, which are needed for the different
# modules. (e.g hpe.oneview and community.vmware vmware.vmware_rest)
# bcl-ov:18


echo "start the ssh-agent like this:"
echo "eval `ssh-agent`
#ssh-add ./bcl_setup_config/cic_aap_labs
ssh-add ./bcl_setup_config/bcl_lab
ssh-add -l
echo podman login quay.io
podman login quay.io

echo podman login registry.redhat.io
podman login registry.redhat.io


echo "set environment by calling:"
echo ". ./bcl_setup_config/env.sh"


Usage: 
# you need a vars file for each use case to create
main vars to define what gets defined:
   inventories2create    - list with names of static inventories which are defined later in more detail   <name>_inventory
       hint: the inventory is created localy for the play itself to work AND later as an AAP static inventory
   organization_name     - the name of the one org which gets created
   ees2create            - list with names ees which are defined later in more detail   <name>_ee
   credtypes2create            - list with names of credential types which are defined later in more detail   <name>_credential_type
   credentials2create            - list with names of credentials which are defined later in more detail   <name>_credential
   projects2create            - list with names of projects which are defined later in more detail   <name>_project
   workflows2create            - list with names of projects which are defined later in more detail   <name>_workflowtemplate
      hint: each workflow will have a list with jobtemplates so the same name is also used for <name>_jobtemplates


# Setting up AAP (including bastion host)
ansible-navigator run bcluc1_setup_esxideploy.yml -e @bcluc1_setup_esxideploy_config/extra_vars.yml -e @bcluc1_setup_esxideploy_config/vault_vars.yml --eei bcl-ov:4 

For all 3 plays you can add:
-e remove=true		removes the artifacts created
-e debug=true		enbles debugging output (not consistently implemented yet)

