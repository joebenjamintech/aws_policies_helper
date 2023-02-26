# AWS Policies Helper

I prefer to create my AWS policies from inside of VIM.

I wrote this script to pull out the services and actions from the [AWS policies file](https://awspolicygen.s3.amazonaws.com/js/policies.js) and use dmenu for the selections.

* This script assumes you have an environment variable by the name of AWS_POLICIES_FILE which points to the location your local policies.txt file.
* This script assumes you have [dmenu](https://tools.suckless.org/dmenu/) set up but you can change this up to use it how you want.
* I also have a vim mapping so I can easily use it in vim.  You can get my vimrc from [here](https://github.com/joedbenjamin/vimrc).
```vim
function! GetAWSServiceActions_Normal()
  let action = system("! awsgetaction")
  let cmd = "\<c-r>='".action."'\<cr>"
  execute 'normal! a'.cmd."\<esc>"
endfunction

function! GetAWSServiceActions_Insert()
  let action = system("! awsgetaction")
  let cmd = "\<c-r>='".action."'\<cr>"
  start
  call feedkeys(cmd) 
endfunction

nnoremap <leader>gs :call GetAWSServiceActions_Normal()<cr>
inoremap <leader>gs <c-o>:call GetAWSServiceActions_Insert()<cr>
```

## Files

* awsgetaction
  * the script that does everything
  * parses the policy file to get all services
  * pipes the services into dmenu for selection
  * takes the selection of the service and uses it to get all the actions related to that service
  * pipes the actions into dmenu for selection
* awsupdatepolicyfile
  * updates the local copy of the policies file
* policies.txt
  * the local copy of the policies file
