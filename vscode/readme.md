Run docker build -t my-alpine-code-server . to build the image.
Run docker run -it -p 8080:8080 my-alpine-code-server to start a container




docker run -d -p 8080:8080 -p 7681:7681 -v vs-code:/workspace vs-code



docker run -d -p 8080:8080 vs-code
