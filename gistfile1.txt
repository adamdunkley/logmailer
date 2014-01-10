#!/bin/sh

# get new shasum
NEW_SHASUM=$(shasum error.log)
NEW_SHASUM=${NEW_SHASUM% *}

# checksum file does not exist, create it, save log state, and exit
if [ ! -f /tmp/error_shasum ]
then
  # save checksum
  echo $NEW_SHASUM > /tmp/error_shasum
  echo "Created a new shasum file."

  # save log state
  cp error.log /tmp/error_backup

  # do nothing this time
  exit
fi

# get old checksum
OLD_SHASUM=$(cat /tmp/error_shasum)

if [ "$NEW_SHASUM" == "$OLD_SHASUM" ]
then
  echo "No changes."
else
  echo "Changes detected:"

  grep -f error.log /tmp/error_backup
fi

exit
