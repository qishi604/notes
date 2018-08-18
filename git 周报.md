# git 周报

git log  --author=seven --pretty=format:"%cd %s" --date=short | awk '$1 >= "2017-05-03" { print }' 