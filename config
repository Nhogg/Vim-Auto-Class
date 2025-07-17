" Automatically generate java classes from filenames
augroup JavaBoilerplate
  autocmd!
  " When a new .java file is created (BufNewFile) and the buffer is empty
  autocmd BufNewFile *.java if expand('%:p') =~# '\.java$' && getline(1) == '' | call JavaBoilerplate() | endif
augroup END

" Function to generate Java class boilerplate
function! JavaBoilerplate()
  let className = expand('%:t:r') " Get filename without extension
  " Remove package declarations (if any) to get just the class name
  let className = substitute(className, '\v^(.*/)*', '', '') 

  " Get package name from directory structure
  " This assumes a standard Maven/Gradle src/main/java/com/example/package/ClassName.java structure
  let fullPath = expand('%:p:h')
  let projectRoot = finddir('.git', fullPath) " Find project root based on .git or .svn, etc.
  if empty(projectRoot)
    let projectRoot = finddir('pom.xml', fullPath) " Try pom.xml for Maven
  endif
  if empty(projectRoot)
    let projectRoot = finddir('build.gradle', fullPath) " Try build.gradle for Gradle
  endif

  let packageName = ''
  if !empty(projectRoot)
    let relativePath = fnamemodify(fullPath, ":~:s?" . projectRoot . "/?\\?gc") " Get relative path from project root
    " Extract package from common Java source roots (src/main/java, src/test/java)
    if relativePath =~# '\v(src/(main|test)/java/)'
      let packageDir = substitute(relativePath, '\v.*(src/(main|test)/java/)\zs(.*)', '\3', '')
      let packageName = substitute(packageDir, '/', '.', 'g')
    endif
  endif

  " Prepare the boilerplate content
  let boilerplate = []
  if !empty(packageName)
    call add(boilerplate, 'package ' . packageName . ';')
    call add(boilerplate, '')
  endif
  " This entire section is modular. If you want to expand this to
  " auto-generate more. just follow the same call add(boilerplate, '') format.
  call add(boilerplate, '/**')
  call add(boilerplate, ' * ' . className . '.java')
  call add(boilerplate, ' *')
  call add(boilerplate, ' * @author Your Name') " Add your author name here
  call add(boilerplate, ' * @version 1.0')
  call add(boilerplate, ' * @since ' . strftime('%Y-%m-%d %H:%M:%S')) " Current date/time
  call add(boilerplate, ' */')
  call add(boilerplate, 'public class ' . className . ' {')
  call add(boilerplate, '')
  call add(boilerplate, '    public static void main(String[] args) {')
  call add(boilerplate, '        // Your code here')
  call add(boilerplate, '    }')
  call add(boilerplate, '}')

  " Insert the boilerplate into the buffer
  call append(0, boilerplate)
  normal! GkJ
  startinsert!
endfunction
