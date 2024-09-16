allow_k8s_contexts('local')
docker_prune_settings(num_builds=1, keep_recent=1)

aissemble_version = '1.9.0-SNAPSHOT'

build_args = { 'DOCKER_BASELINE_REPO_ID': 'ghcr.io/',
               'VERSION_AISSEMBLE': aissemble_version}

# Kafka
yaml = helm(
    'test-311-deploy/src/main/resources/apps/kafka-cluster',
    values=['test-311-deploy/src/main/resources/apps/kafka-cluster/values.yaml',
        'test-311-deploy/src/main/resources/apps/kafka-cluster/values-dev.yaml']
)
k8s_yaml(yaml)

# Add deployment resources here



# For WSL users, the configuration files need to be in an accessible path. Update the project path to the root file system. Example: '/mnt/c' or '/mnt/wsl/rancher-desktop'
project_path = os.path.abspath('.')
# Update configuration_files_path to match the path of the config files to be loaded into the configuration store. Example 'my-project-deploy/src/main/resources/config'
configuration_files_path = 'src/main/resources/configurations'

load('ext://helm_resource', 'helm_resource')
helm_resource(
    name='configuration-store',
    release_name='configuration-store',
    chart='test-311-deploy/src/main/resources/apps/configuration-store',
    namespace='config-store-ns',
    flags=['--values=test-311-deploy/src/main/resources/apps/configuration-store/values.yaml',
           '--values=test-311-deploy/src/main/resources/apps/configuration-store/values-dev.yaml',
           '--set=aissemble-configuration-store-chart.configurationVolume.volumePathOnNode=' + project_path + '/' + configuration_files_path,
           '--create-namespace']
)

yaml = helm(
   'test-311-deploy/src/main/resources/apps/spark-operator',
   name='spark-operator',
   values=['test-311-deploy/src/main/resources/apps/spark-operator/values.yaml',
       'test-311-deploy/src/main/resources/apps/spark-operator/values-dev.yaml']
)
k8s_yaml(yaml)
k8s_kind('SparkApplication', image_json_path='{.spec.image}')

yaml = helm(
   'test-311-deploy/src/main/resources/apps/policy-decision-point',
   name='policy-decision-point',
   values=['test-311-deploy/src/main/resources/apps/policy-decision-point/values.yaml',
       'test-311-deploy/src/main/resources/apps/policy-decision-point/values-dev.yaml']
)
k8s_yaml(yaml)
# For WSL users, the configuration files need to be in an accessible path. Update the project path to the root file system. Example: '/mnt/c' or '/mnt/wsl/rancher-desktop'
project_path = os.path.abspath('.')
# Update configuration_files_path to match the path of the config files to be loaded into the configuration store. Example 'my-project-deploy/src/main/resources/config'
configuration_files_path = ''

load('ext://helm_resource', 'helm_resource')
helm_resource(
    name='configuration-store',
    release_name='configuration-store',
    chart='test-311-deploy/src/main/resources/apps/configuration-store',
    namespace='config-store-ns',
    flags=['--values=test-311-deploy/src/main/resources/apps/configuration-store/values.yaml',
           '--values=test-311-deploy/src/main/resources/apps/configuration-store/values-dev.yaml',
           '--set=aissemble-configuration-store-chart.configurationVolume.volumePathOnNode=' + project_path + '/' + configuration_files_path,
           '--create-namespace']
)

# spark-worker-image
docker_build(
    ref='test-311-spark-worker-docker',
    context='test-311-docker/test-311-spark-worker-docker',
    build_args=build_args,
    extra_tag='test-311-spark-worker-docker:latest',
    dockerfile='test-311-docker/test-311-spark-worker-docker/src/main/resources/docker/Dockerfile'
)


yaml = helm(
   'test-311-deploy/src/main/resources/apps/pipeline-invocation-service',
   name='pipeline-invocation-service',
   values=['test-311-deploy/src/main/resources/apps/pipeline-invocation-service/values.yaml',
       'test-311-deploy/src/main/resources/apps/pipeline-invocation-service/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-311-deploy/src/main/resources/apps/spark-infrastructure',
   name='spark-infrastructure',
   values=['test-311-deploy/src/main/resources/apps/spark-infrastructure/values.yaml',
       'test-311-deploy/src/main/resources/apps/spark-infrastructure/values-dev.yaml']
)
k8s_yaml(yaml)
k8s_yaml('test-311-deploy/src/main/resources/apps/spark-worker-image/spark-worker-image.yaml')


yaml = helm(
   'test-311-deploy/src/main/resources/apps/s3-local',
   name='s3-local',
   values=['test-311-deploy/src/main/resources/apps/s3-local/values.yaml',
       'test-311-deploy/src/main/resources/apps/s3-local/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-311-deploy/src/main/resources/apps/metadata',
   name='metadata',
   values=['test-311-deploy/src/main/resources/apps/metadata/values.yaml',
       'test-311-deploy/src/main/resources/apps/metadata/values-dev.yaml']
)
k8s_yaml(yaml)


# Add deployment resources here