# VSEE

#### 🔌 Visual Studio Extension Environment

Creates a dockerized "yo" command with `sudo ./build-yo-code-docker-image` in
a docker image named "yo-code".

Then create a "yo" alias with
`alias yo='sudo docker run -i -t -v "${PWD}:/mnt/host" yo --no-insight"` and
now you can run `yo code`.

That's it.
