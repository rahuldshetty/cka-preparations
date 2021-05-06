# Rollout & Rollback

New Deploy -> New Rollout -> new version #1

New Deploy -> New Rollout -> new version #2

Check status: `kubectl rollout status deploy mydeploy`
Check history: `kubectl rollout history deploy mydeploy`

Deployment Strategy:
#1 Recreate [Delete all & Create new one]
#2 RollingUpdate [Blue/Green Deployment]

Do change: `kubectl set image deploy mydeploy <container|nginx>=nginx:1.19.4`
Undo Change: `kubectl rollout undo deploy mydeploy`


# Command & Arguments



