# Educates Carvel Package


## Steps to create a new version

1. Update the new version to vendir
   
   edit `vendir.yml` and update line 10 to your new version
   ```
          ref: 21.04.27.1
   ```

2. vendir sync the new sources
   
   ```
   vendir sync
   ```

3. lock images
   ```
   kbld -f images.yaml --imgpkg-lock-output .imgpkg/images.yml
   ```

   __NOTE__: If images are referenced in upstream descriptors
   ```
   kbld -f . --imgpkg-lock-output .imgpkg/images.yml
   ```

4. test overlays
   ```
   ytt -f config
   ```

5. Build bundle
   ```
   # imgpkg copy --bundle quay.io/eduk8s/educates-operator-package:latest 
   ```

   Run a local docker registry:
   ```
   docker run -d -p 5000:5000 --restart=always --name registry registry:2
   docker run -it --rm -p 5000:5000 --name registry registry:2
   ```
   
   __NOTE__: If we want to push the bundle
   ```
   imgpkg push --bundle localhost:5000/eduk8s/educates-operator-package:latest --file bundle
   # imgpkg push --bundle quay.io/eduk8s/educates-operator-package:latest --file bundle
   ```