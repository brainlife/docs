
# Projects

Project contains your datasets, executed processes, publication records, and pipeline rules.

Each project can be set to use either `public` or `private` access policy, and you can define a group of users to be either `admins`, `members` or both.  Only the project `administrators` are allowed to update the project access policy, and edit `admins` and `members` of the project.

`Private` project allows only the project members to download, or use the datasets to process data. Only the admins and project members can find the project metadata.

`Public` project allows anyone registered on Brainlife to download and use the datasets to process data. 


Following table show who can perform which action under Public and Private project.

| Action | Public Project | Private Project |
| ------------- | ------------- | ----- |
| Update project detail | Admin | Admin |
| See project detail | Admin / Members | Admin / Members |
| List datasets | Admin / Members | Admin / Members |
| Download datasets | Admin / Members | Admin / Members |
| Update dataset detail | Admin / Members | Admin / Members |
| Create publication record | Admin / Members | Admin / Members |
| Update publication record | Admin / Members | Admin / Members |
| Upload datasets | Admin / Members | Admin / Members |
| List processes | Admin / Members | Admin / Members |
| Submit new process | Admin / Members | Admin / Members |
| Access process output | Admin / Members | Admin / Members |
| List published datasets | Admin / Members / Guest | Admin / Members / Guest |
| List publication records | Admin / Members / Guest | Admin / Members / Guest |
| Download published datasets | Admin / Members / Guest | Admin / Members / Guest |

## Dataset Archive

Once you store your dataset on project dataset page, they will be stored in Brainlife `archive` and be stored until you remove them. Once you store your dataset, the content of the data (the raw files) can not be modified, although you can still update the description, metadata, and tags. When you submit a process, the datasets from the archive will be automatically staged out of the archive to the remote computing resources that you have access to.

!!! note
    When you create a publications record, datasets that are part of the publication will be copied to a secure tape archive (IU SDA/HPSS system)
    for added data security. 


