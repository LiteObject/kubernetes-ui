Kubernetes UI:
0. 	Source: https://github.com/kubernetes/dashboard
1.	To deploy Dashboard, execute following command:
	$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml
2.	To access Dashboard from your local workstation you must create a secure channel to your Kubernetes cluster. Run the following command:
	$ kubectl proxy
3. 	URL to access the dashboard:
	http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
4. 	Creating sample user:
		CreateServiceAccount.yaml
		
			apiVersion: v1
			kind: ServiceAccount
			metadata:
			 name: admin-user
			 namespace: kubernetes-dashboard
			
		$kubectl apply -f CreateServiceAccount.yml
		
		CreateClusterRoleBinding.yaml
		
			apiVersion: rbac.authorization.k8s.io/v1
			kind: ClusterRoleBinding
			metadata:
			  name: admin-user
			roleRef:
			  apiGroup: rbac.authorization.k8s.io
			  kind: ClusterRole
			  name: cluster-admin
			subjects:
			- kind: ServiceAccount
			  name: admin-user
			  namespace: kubernetes-dashboard
			  
		$kubectl apply -f CreateClusterRoleBinding.yml
		
5.	Getting a Bearer Token:
	kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
	
	eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLXY1N253Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIwMzAzMjQzYy00MDQwLTRhNTgtOGE0Ny04NDllZTliYTc5YzEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.Z2JrQlitASVwWbc-s6deLRFVk5DWD3P_vjUFXsqVSY10pbjFLG4njoZwh8p3tLxnX_VBsr7_6bwxhWSYChp9hwxznemD5x5HLtjb16kI9Z7yFWLtohzkTwuFbqmQaMoget_nYcQBUC5fDmBHRfFvNKePh_vSSb2h_aYXa8GV5AcfPQpY7r461itme1EXHQJqv-SN-zUnguDguCTjD80pFZ_CmnSE1z9QdMHPB8hoB4V68gtswR1VLa6mSYdgPwCHauuOobojALSaMc3RH7MmFUumAgguhqAkX3Omqd3rJbYOMRuMjhANqd08piDC3aIabINX6gP5-Tuuw2svnV6NYQ
	
6. Remove the admin ServiceAccount and ClusterRoleBinding:
	$kubectl -n kubernetes-dashboard delete serviceaccount admin-user
	$kubectl -n kubernetes-dashboard delete clusterrolebinding admin-user
	
	