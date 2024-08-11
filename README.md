_2024-08-11  18:44_

*To check if the container works well separately*
```bash
docker build -t azw_app . && docker run --rm -d --name azw_container -p 3000:3000 azw_app
```


**To run the GitHun Action:**

[1] - Run the bash snippet
```bash
git add .
git commit -m "My awesome commit."
git push -u origin main
```

[2] - Check in https://github.com/Azrubael/EPAM-CICD-Lab1/actions/