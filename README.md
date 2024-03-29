#Sonar Cloud

1) Create Custom Quality Gate in SonarCloud and Add conditions to the Quality Gate
2) Assign this Quality Gate to the Project
3) Add script in azure-pipeline.yaml file to enable quality gate check (Note: This will fail your build in case Quality Gate fails)

 #for Quality gates add under the script


1) sudo apt-get update
2) sudo apt-get -y install curl jq
3) mvn verify package sonar:sonar -Dsonar.host.url=https://sonarcloud.io/ -Dsonar.organization=azuredevopsdevsecopsoorg -Dsonar.projectKey=azuredevsecopsoprojectkey -Dsonar.token=d268968115b4935bcd007ff54f01906c18e33cb6
4) sleep 5
5) quality_status=$(curl -s -u d268968115b4935bcd007ff54f01906c18e33cb6: https://sonarcloud.io/api/qualitygates/project_status?projectKey=azuredevsecopsoprojectkey | jq -r '.projectStatus.status')
6) echo "SonarCloud analysis status is $quality_status"; 
7) if [[ $quality_status == "ERROR" ]] ; then exit 1;fi
8) displayName: "Integrate SAST using SonarCloud to populate code coverage in Azure DevOps DevSecOps Pipeline"


-----------Sample JSON Response from SonarCloud or SonarQube Quality Gate API---------------------

{
	"projectStatus": {
		"status": "ERROR",
		"conditions": [
			{
				"status": "ERROR",
				"metricKey": "coverage",
				"comparator": "LT",
				"errorThreshold": "90",
				"actualValue": "0.0"
			}
		],
		"periods": [],
		"ignoredConditions": false
	}
}
