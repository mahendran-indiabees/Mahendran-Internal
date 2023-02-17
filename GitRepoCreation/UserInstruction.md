## This document helps to create Github Repo using Jenkins

###### Prerequisite
```
Jenkins
Github Cli Package
```

###### Create the Simple Pipeline Job in Jenkins and add below snippet in Pipeline script
```
node
{
	try {
			stage('Validate Parameters')
			{
				if(RepoName == null || RepoName == "")
				{
					error("Repo Name should not be empty")
				}
				if(RepoDescription == null || RepoName == "")
				{
					error("Repo Description should not be empty")
				}
				echo "GitHub Repo Name : "+RepoName
				echo "GiHub Repo Type : "+RepoType
				echo "Repo Description : "+RepoDescription
			}
			stage('Repo Creation')
			{
				withCredentials([string(credentialsId: 'GitHub_Token', variable: 'Github_Credentials')]) {
					env.GH_TOKEN=Github_Credentials     
					bat 'gh repo create '+RepoName+' --'+RepoType+' --description '+'"'+RepoDescription+'"'+' --add-readme'
				}
			}
	}
	catch(Exception ex)
	{
		currentBuild = 'FAILURE'
		throw ex		
	}
	finally
	{
		echo "API call to update the status of Jenkins result in Service Now"
	}
}
```
###### Provide the GitHub Repo, Description & Repo Type in Build with Parameters Option
![image](https://user-images.githubusercontent.com/96326288/219654765-a7dae4fa-7846-46d4-856c-dadd08cba9ca.png)

###### Console Output looks like below
![image](https://user-images.githubusercontent.com/96326288/219655534-48d78ed8-82b1-46a6-9a3d-acca3be865e8.png)


###### Repo has been created in GitHub
![image](https://user-images.githubusercontent.com/96326288/219655798-9f7a9686-fcad-4f44-832a-8b7f9e7276cb.png)


