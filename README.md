# golf-scorecard-app
cd ~/golf-scorecard-app
git status
git remote remove origin 2>/dev/null
git remote add origin https://github.com/aiconfirmedd/golf-scorecard-app.git
git branch -M main
git add .
git commit -m "chore: final project sync before deploy" || true
git push -u origin main
