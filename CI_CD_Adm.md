CI/CD Administration in Databricks

![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)

 ## Demo: Setting up Databricks Git Folders

 setting the git on databricks:
1. Click on databricks logo
1. Click on user menu (top left conner) ![alt text](image-10.png)
1. Follow on: Linked accounts > Git provider > Personal access token > git username > token > save
![alt text](image-11.png)
1. Select the HTTPS url of your folder
![alt text](image-12.png)

1. Workspace > Create > Git folder
![alt text](image-13.png)
1. URL > Git provider > Git folder name > Create Git Folder
![alt text](image-14.png)
...
![alt text](image-15.png)

In granting access to Databricks Git Folder, keep in mind that permissions apply to the entire Databricks Git Folder.

* View: provides read-only access to Databricks Git Folder

* Run: View + run

* Edit: View + run + modify existing files

* Manage: Provides complete administrative capabilities over Databricks Git Folder

Setting permissions on Git Folder:
![alt text](image-16.png)

