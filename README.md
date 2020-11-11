# Tic-tac-toe react app with local molecule testing and github actions

Using KinD (kubernetes in docker), we can run local testing for kubnetes deployment.
So that we can leverage this in any CICD and automate code push to deployment cycle.

I will try to demo this in github actions as demo pipeline run.

1. Passing this molecule test, we can push the new image to docker registry.

2. Then, apply to any cloud (Azure, AWS, GCP) with passing CICD status.


### Stage 1 --> Local testing 
1. Build optimised npm packages
```bash
npm run-script build
```
2. Create Dockerfile file
```
FROM nginx:1.19.0
COPY build/ /usr/share/nginx/html
```

3. Create .dockerignore to prevent copying large node_modules folder into image

4. Push image to local docker registry
```bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

### Stage 2 --> Local testing with kind and molecule
1. brew install kind docker

2. pip install -r requirements.txt

3. molecule test -s default

4. Fix weird kinD behavior in mac
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/#tls-certificate-errors


### Stage 3 --> Push react app docker image to registry
5. Build and tag the image for pushing to docker hub registry
```bash
docker build -t tic-tac-toe .
docker tag tic-tac-toe andeslam2019/tic-tac-toe:latest
docker push andeslam2019/tic-tac-toe:latest
```

### Stage 4 (Final) --> Deploy to aws (Example)
1. export KUBECONFIG=~/.kube/eks-andes
2. Using github secrets

Deploy this new image in k8s
```bash
kubectl apply -f deploy.yml
```


## Debug kubernetes commands

1. Check for any error in current kubernetes context
```bash
kubectl version
kubectl config current-context
kubectl config 
```

2. Check for any error in deployment
```bash
kubectl describe deployment <deployment_name>
```

3. Check for any pods, services
```bash
kubectl get svc / kubectl get service
kubectl get pods
```

4. Check for current cluster info
```bash
kubectl cluster-info
```

# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
