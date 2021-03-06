VERSION = 0.0.2
NAME = sealed-secrets-operator-helm

RELEASE_IMAGE_0_0_2=quay.io\/disposab1e\/sealed-secrets-operator-helm@sha256:d07a66107243a0054a54aac7cd71c0a8a342c4cd45c6609ec2ab6470df16f09c

REGISTRY_REL = quay.io
REGISTRY_DEV = imac.fritz.box:5000
REGISTRY_CI = docker.io
NAMESPACE=disposab1e

BUNDLE_MANIFEST_BASE_DIR = ../deploy/olm-catalog/
BUNDLE_DIR = bundle
BUNDLE_BUILD_BASE_DIR = _output/bundle
GUIDES_BUILD_BASE_DIR = _output/guides



########################################################
# ALL - Development & Continuous Integration Environment
########################################################

########################################################
# ALL - Clean
########################################################

clean:
	rm -rf _output


########################################################
# ALL - Documentation
########################################################

docs-genrate:
	cd ../docs && make html

docs-view:
	cd ../docs && make html && open ../docs/build/html/index.html


########################################################
# ALL - Lint Helm templates
########################################################

helm-templates:
	helm lint ./../helm-charts/sealed-secrets/
	helm template --values ./../helm-charts/sealed-secrets/values.yaml  ./../helm-charts/sealed-secrets/
	helm template --values ./../examples/k8s.yaml ./../helm-charts/sealed-secrets
	helm template --values ./../examples/ocp.yaml ./../helm-charts/sealed-secrets


########################################################
# CI/CD - Continuous Integration Environment
########################################################

########################################################
# CI/CD - Operator
########################################################

operator-build-ci:
	export GO111MODULE=on
	cd ../ && operator-sdk build ${REGISTRY_CI}/${NAMESPACE}/${NAME}:v${VERSION}
	
operator-push-ci:
	docker push ${REGISTRY_CI}/${NAMESPACE}/${NAME}:v${VERSION}


########################################################
# CI/CD - Bundle - k8s
########################################################

bundle-generate-k8s-ci:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/ci/k8s
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/k8s/${NAME} ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml

bundle-verify-k8s-ci:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/ci/k8s/manifests/${NAME}

bundle-build-k8s-ci: 
	docker build -t ${REGISTRY_CI}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/ci/k8s

bundle-push-k8s-ci:
	docker push ${REGISTRY_CI}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION}


########################################################
# CI/CD - Bundle - ocp
########################################################

bundle-generate-ocp-ci:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/ci/ocp
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/ocp/${NAME} ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml

bundle-verify-ocp-ci:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/ci/ocp/manifests/${NAME}

bundle-build-ocp-ci: 
	docker build -t ${REGISTRY_CI}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/ci/ocp

bundle-push-ocp-ci:
	docker push ${REGISTRY_CI}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION}


########################################################
# CI/CD - Guides - k8s
########################################################

guide-generate-k8s-manual-ci:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual/crd.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ../deploy/common/deployment.yaml > ${GUIDES_BUILD_BASE_DIR}/ci/k8s/manual/deployment.yaml

guide-generate-k8s-olm-ci:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm
	cp ../deploy/k8s/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ../deploy/k8s/catalog-source.yaml > ${GUIDES_BUILD_BASE_DIR}/ci/k8s/olm/catalog-source.yaml


########################################################
# CI/CD - Guides - ocp3
########################################################

guide-generate-ocp3-manual-ci:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual/crd.yaml
	echo "---" >> ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual/role-binding.yaml
	cat ../deploy/ocp3/role-binding.yaml >> ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual/role-binding.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ../deploy/common/deployment.yaml > ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/manual/deployment.yaml

guide-generate-ocp3-olm-ci:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm
	cp ../deploy/ocp3/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ../deploy/ocp3/catalog-source.yaml > ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm/catalog-source.yaml
	# cluster-admin to service account cause bug in openshift
	cp ../deploy/ocp3/role-binding.yaml  ${GUIDES_BUILD_BASE_DIR}/ci/ocp3/olm




########################################################
# DEV - Development Environment
########################################################

########################################################
# DEV - Operator
########################################################

operator-build-dev:
	export GO111MODULE=on
	cd ../ && operator-sdk build ${REGISTRY_DEV}/${NAMESPACE}/${NAME}:v${VERSION}
	
operator-push-dev:
	docker push ${REGISTRY_DEV}/${NAMESPACE}/${NAME}:v${VERSION}

########################################################
# DEV - Bundle - k8s
########################################################

bundle-generate-k8s-dev:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/dev/k8s
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/dev/k8s/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/dev/k8s/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/k8s/${NAME} ${BUNDLE_BUILD_BASE_DIR}/dev/k8s/manifests/

bundle-verify-k8s-dev:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/dev/k8s/manifests/${NAME}

bundle-build-k8s-dev: 
	docker build -t ${REGISTRY_DEV}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/dev/k8s/

bundle-push-k8s-dev:
	docker push ${REGISTRY_DEV}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION}


########################################################
# DEV - Bundle - ocp
########################################################

bundle-generate-ocp-dev:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/dev/ocp
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/dev/ocp/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/dev/ocp/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/ocp/${NAME} ${BUNDLE_BUILD_BASE_DIR}/dev/ocp/manifests/

bundle-verify-ocp-dev:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/dev/ocp/manifests/${NAME}

bundle-build-ocp-dev: 
	docker build -t ${REGISTRY_DEV}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/dev/ocp/

bundle-push-ocp-dev:
	docker push ${REGISTRY_DEV}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION}


########################################################
# DEV - Guides - k8s
########################################################

guide-generate-k8s-manual-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/common/deployment.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/manual/crd.yaml

guide-generate-k8s-olm-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm
	cp ../deploy/k8s/catalog-source.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm
	cp ../deploy/k8s/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/dev/k8s/olm


########################################################
# DEV - Guides - ocp3
########################################################

guide-generate-ocp3-manual-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/common/deployment.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual/crd.yaml
	echo "---" >> ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual/role-binding.yaml
	cat ../deploy/ocp3/role-binding.yaml >> ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/manual/role-binding.yaml

guide-generate-ocp3-olm-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	cp ../deploy/ocp3/catalog-source.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	cp ../deploy/ocp3/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm
	# cluster-admin to service account cause bug in openshift
	cp ../deploy/ocp3/role-binding.yaml  ${GUIDES_BUILD_BASE_DIR}/dev/ocp3/olm

########################################################
# DEV - Guides - ocp4
########################################################

guide-generate-ocp4-manual-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/common/deployment.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/manual/crd.yaml

guide-generate-ocp4-olm-dev:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm
	cp ../deploy/ocp4/catalog-source.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm
	cp ../deploy/ocp4/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/dev/ocp4/olm


########################################################
# RELEASE - Release Environment
########################################################

########################################################
# RELEASE - Operator
########################################################

operator-build-release:
	export GO111MODULE=on
	cd ../ && operator-sdk build ${REGISTRY_REL}/${NAMESPACE}/${NAME}:v${VERSION}
	
operator-push-release:
	docker push ${REGISTRY_REL}/${NAMESPACE}/${NAME}:v${VERSION}


########################################################
# RELEASE - Bundle - k8s
########################################################

bundle-generate-k8s-release:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/release/k8s
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/release/k8s/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/k8s/${NAME} ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	sed 's/${REGISTRY_DEV}\/${NAMESPACE}\/${NAME}:v${VERSION}/${RELEASE_IMAGE_0_0_2}/g' ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
#	cp -r ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME} ../deploy/operatorhubs/k8s

bundle-verify-k8s-release:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/release/k8s/manifests/${NAME}

bundle-build-k8s-release:
	docker build -t ${REGISTRY_REL}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/release/k8s/

bundle-push-k8s-release:
	docker push ${REGISTRY_REL}/${NAMESPACE}/${NAME}-registry-k8s:${VERSION}


########################################################
# RELEASE - Bundle - ocp
########################################################

bundle-generate-ocp-release:
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/release/ocp
	cp -r ${BUNDLE_DIR}/* ${BUNDLE_BUILD_BASE_DIR}/release/ocp/
	mkdir -p ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests
	cp -r ${BUNDLE_MANIFEST_BASE_DIR}/ocp/${NAME} ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/
	sed 's/${REGISTRY_DEV}/${REGISTRY_CI}/g' ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/0.0.1/_sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/0.0.1/sealed-secrets-operator-helm.v0.0.1.clusterserviceversion.yaml
	sed 's/${REGISTRY_DEV}\/${NAMESPACE}\/${NAME}:v${VERSION}/${RELEASE_IMAGE_0_0_2}/g' ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml > ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	rm ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
	mv ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/${VERSION}/_sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}/${VERSION}/sealed-secrets-operator-helm.v${VERSION}.clusterserviceversion.yaml
#	cp -r ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME} ../deploy/operatorhubs/ocp

bundle-verify-ocp-release:
	operator-courier --verbose verify --ui_validate_io ${BUNDLE_BUILD_BASE_DIR}/release/ocp/manifests/${NAME}

bundle-build-ocp-release: 
	docker build -t ${REGISTRY_REL}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION} ${BUNDLE_BUILD_BASE_DIR}/release/ocp/

bundle-push-ocp-release:
	docker push ${REGISTRY_REL}/${NAMESPACE}/${NAME}-registry-ocp:${VERSION}


########################################################
# RELEASE - Guides - k8s
########################################################

guide-generate-k8s-manual-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual/crd.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/common/deployment.yaml > ${GUIDES_BUILD_BASE_DIR}/release/k8s/manual/deployment.yaml

guide-generate-k8s-olm-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm
	cp ../deploy/k8s/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/k8s/catalog-source.yaml > ${GUIDES_BUILD_BASE_DIR}/release/k8s/olm/catalog-source.yaml

########################################################
# RELEASE - Guides - ocp3
########################################################

guide-generate-ocp3-manual-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual/crd.yaml
	echo "---" >> ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual/role-binding.yaml
	cat ../deploy/ocp3/role-binding.yaml >> ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual/role-binding.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/common/deployment.yaml > ${GUIDES_BUILD_BASE_DIR}/release/ocp3/manual/deployment.yaml

guide-generate-ocp3-olm-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm
	cp ../deploy/ocp3/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/ocp3/catalog-source.yaml > ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm/catalog-source.yaml
	# cluster-admin to service account cause bug in openshift
	cp ../deploy/ocp3/role-binding.yaml  ${GUIDES_BUILD_BASE_DIR}/release/ocp3/olm

########################################################
# RELEASE - Guides - ocp4
########################################################

guide-generate-ocp4-manual-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	cp ../deploy/common/service-account.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	cp ../deploy/common/role.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	cp ../deploy/common/role-binding.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual
	cp ../deploy/crds/bitnami.com_sealedsecretcontrollers_crd.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual/crd.yaml
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/common/deployment.yaml > ${GUIDES_BUILD_BASE_DIR}/release/ocp4/manual/deployment.yaml

guide-generate-ocp4-olm-release:
	rm -rf ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm
	mkdir -p ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm
	cp ../deploy/common/namespace.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm
	cp ../deploy/common/operator-group.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm
	cp ../deploy/ocp4/subscription.yaml ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm
	sed 's/${REGISTRY_DEV}/${REGISTRY_REL}/g' ../deploy/ocp4/catalog-source.yaml > ${GUIDES_BUILD_BASE_DIR}/release/ocp4/olm/catalog-source.yaml

