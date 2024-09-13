# dccu

```bash
######
## DCCU
####

dccu () {
  if [ $# -eq 0 ]; then
    if [ ${#DCC_FLAGS[*]} -eq 0 ]; then
      echo "NO DCC_FLAGS\!"
      return 1
    fi
  else
    DCC_FLAGS=($@)
  fi

  DCC_CONTEXT=()

  counter=0

  for dccFlagIndex in ${!DCC_FLAGS[*]}; do
    DCC_DIRECTORY=${DCC_FLAGS[$dccFlagIndex]}
    if [ -d ${DCC_DIRECTORY} ]; then
      DCC_DEFINITION=${DCC_DIRECTORY}/docker-compose.yml
      if [ -f ${DCC_DEFINITION} ]; then
        DCC_CONTEXT[$((counter++))]="-f ${DCC_DEFINITION}"
        iCounter=0
        while [ $iCounter -lt ${#DCC_FLAGS[*]} ]; do
          DCC_EXTENSION=${DCC_DIRECTORY}/docker-compose.${DCC_FLAGS[$((iCounter++))]}.yml
          if [ -f ${DCC_EXTENSION} ]; then
            DCC_CONTEXT[$((counter++))]="-f ${DCC_EXTENSION}"
          fi
        done
      fi
    fi
  done

  export DCC_FLAGS
  export DCC_CONTEXT

  echo ${DCC_CONTEXT[*]}
}
```
