SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.marygabry.name/library/test-app-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'test-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null " +
              "&& kubectl get workload test-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    image_selector='harbor.marygabry.name/library/test-app' + NAMESPACE,
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('test-app', port_forwards=["8080:8080"],extra_pod_selectors=[{'serving.knative.dev/service': 'test-app'}])
allow_k8s_contexts('arn:aws:eks:us-east-2:083459807443:cluster/tap')