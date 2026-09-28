# `step03`: Create MFT SaaS user, virtual folder and reception rule. Test using WebClient

## Move Repository to `step03`

If you are following the tutorial sequentially using the sandbox container from [`step01`](step01.md), switch the repository to the `step03` tag/branch before proceeding.

Inside the `1c-mft-example` sandbox shell (or using `lazygit` under the **Tags** or **Remotes** tab):

```sh
git checkout step03
```

## Create User

Open the MFT capability in the SaaS tenant:

![Open MFT](../img/03.01.OpenMFTSaaS.png)

Observe the menu on the left and open the user management section:

![Open User Management](../img/03.02.OpenUsersSection.png)

Add a new user.

![Add user](../img/03.03.AddUser.png)

This operation will add a user to the local users database and this user is only recognized by MFT, not Integration Server. Fill in the form and click `Add to user list`, then click on `Create`. Note that multiple users can be added in a single `Create` operation.

![User Creation Details](../img/03.04.AddUserTutorial01.png)

## Create Virtual Folder and Attach the User with All Permissions

Once the user is created, also create a local virtual folder for them. Go to `Virtual folders`:

![Virtual Folders](../img/03.05.VirtFolders.png)

Then click on the plus icon:

![Plus Icon Add Virtual folder](../img/03.06.PlusVirtFolder.png)

Name the folder `tutorial.1.home`:

![Give Name and Add](../img/03.07.NameAndAdd.png)

Immediately after clicking `Add`, configure the virtual folder to use the default location, write manually `home/tutorial.1` in the `Browse folder` field, then expand the `Permissions` subsection:

![First details on VFS](../img/03.08.VirtualFolderDetails01.png)

Note: the path `home/tutorial.1` is entered manually because virtual folders map onto a physical location under a hidden base directory managed by the service.

> ⚠️ **Advanced:** Virtual folder paths are independent of their names, so overlaps are possible — for example, a folder named `home` pointing to `home/` would physically contain any subfolder also mapped to another virtual folder. Keep path assignments consistent to avoid unintended sharing.


Add the user we created in the previous step:

![Add user to VFS plus](../img/03.09.AddUserToVFS.png)

![Search and add user](../img/03.10.SearchAndAddUser.png)

Give all permissions, then save.

![Add permissions and save](../img/03.11.AddPermissionsAndSave.png)

## Verify the User Via the WebClient

WebClient is a web browser application that allows a user to manipulate files within the associated virtual folders.

The WebClient URL can be found in the Listeners section.

![HTTPS listener](../img/03.12.WebClientHttpsListener.png)

Use the copy button on an HTTP/S listener and open a new browser tab, then paste the URL.

![WebClient Login](../img/03.13.WebClientLogIn.png)

After successfully logging in, create a new folder called `inbound`. We will use it later.

![Create Folder](../img/03.14.CreateFolder.png)

![Name The New Folder](../img/03.15.CreateInboundFolder.png)

![Folder Created](../img/03.16.FolderCreated.png)

The WebClient lets you browse the contents of virtual folders, whereas the administration interface only shows virtual file system configuration.

## Create a First Rule

First, let's prepare an image that we can upload to the inbound folder. Just use any image or small file, if no file is available use one from the internet, like this one:

![IBM Logo](https://www.ibm.com/content/dam/connectedassets-adobe-cms/worldwide-content/arc/cf/ul/g/4e/19/IBMLogo_%20current.jpg)

Save the image in a local folder for the moment.

Now let's return to the administration UI and add a post-processing rule. Open the `Post-Processing Actions` section:

![Post-Processing Actions](../img/03.17.OpenPostProcessingRules.png)

Add a new rule:

![New Rule](../img/03.18.NewRule.png)

Give the name `tutorial.1.action` and click on `Criteria`:

![Name and Trigger Criteria](../img/03.19.NameAndTrigger.png)

Configure the trigger criteria for `Upload` in the `Specific folder` `/tutorial.1.home/inbound/`:

![Trigger on inbound folder Upload](../img/03.20.TriggerOnInboundFolderUpload.png)

Add a task:

![Add Task](../img/03.21.AddTask.png)

Select `Rename` and then write `{name}.{md5}.received` in the `New file name` field. Scroll up until you see the `Active` button and activate the rule, then click `Add` to save the rule.

Note: `{name}` and `{md5}` are server variables that MFT provides automatically. See [documentation](https://www.ibm.com/docs/en/wm-mft?topic=actions-server-variables) for details.

![Set up Rename Rule](../img/03.22.SetUpRenameRule.png)

After adding the rule, go back to the post-processing rules page and you should see the rule activated.

![See the activated rule](../img/03.23.RuleActivated.png)

Now switch back to the WebClient and upload the image file we prepared into the `inbound` folder.

![Upload File](../img/03.24.UploadTestFile.png)

![Upload Detailed Screen](../img/03.25.UploadDetailedScreen.png)

After upload, close the details popup window and note the file in the folder:

![File uploaded](../img/03.26.FileUploaded.png)

Click the refresh button to the right of `mft.tutorial.user.01/inbound` folder breadcrumb:

![File Renamed](../img/03.27.NoteFileNameAfterRefresh.png)

You can observe the file was renamed.

This concludes the tutorial's third step.

**Side note**: At the moment of this tutorial writing, the server variable {md5} actually contains a sha256 checksum. This is visible in the renamed filename. If you think this error should be corrected, please vote the IBM Idea [WEBMMFTS-I-51](https://ideas.ibm.com/ideas/WEBMMFTS-I-51).

---

← Previous: [`step02`](step02.md) | [Back to README](../README.md)