# Essentials commands for linux


## This file is used for essentials commands which are related to general management and not specific scopes




## Files

### Find


```
# simple search by name
find . -name "data.txt"

# simple search by size
find . -size +1G/B/K

# simple find by modification time
find . -mmin -5

# simple find by change min (change ownership or metadata)
find . -cmin -5

# find by permission
find . -perm 777

# find by 
```