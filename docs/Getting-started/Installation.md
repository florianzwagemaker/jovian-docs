# Installation instructions

The installation process of Jovian is made to be as easy as possible.  
Depending on your preferences, the installation can be done automatically with a single command or through manual individual commands.  

For inexperienced users, we **strongly** recommend you to use the automated installer.

## Downloading the package

The command below will download the latest Jovian release, places it in a directory named `Jovian` and will automatically navigate to this newly made directory.

```bash
git clone https://github.com/DennisSchmitz/Jovian.git; cd Jovian
```


If you wish to use Jovian on a per-project basis (recommended), then use the snippet below and change `#!bash "<project>"` to the name of your desired project. This will automatically navigate you to the newly made directory.
```bash
dir="<project>"; git clone https://github.com/DennisSchmitz/Jovian.git $dir; cd $dir
```

## Automatic installation process (recommended)

The automated installer will go through the installation process of basic Jovian dependencies and will install them for you, as well as the required databases. You'll only have to go through this process once.  

The entire installation process will take (depending on the speed of your computer) about an hour or two. There are a few manual interactions where you will be asked to confirm an action or to make a choice of options.  

Once the installer is busy with downloading and installing the required databases then it's fine to leave your pc as this step will take a long time without manual interventions.

Start the installer with the following command:

```bash
bash jovian -id
```

You can skip the various confirmation-prompts by using the `-y` flag. This will make sure all installation steps will simply execute without you having to confirm.

```bash
bash jovian -id -y
```


## Manual installation

!!! Warning "Please only perform a manual installation if you're an experienced Linux user."

!!! note
    TBA