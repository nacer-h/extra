;; Install MELPA Package
;;(when (>= emacs-major-version 24)
  (require 'package)
  (add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
;; (package-initialize)
;;)

;; Install MARMALADE Package
;;(when (>= emacs-major-version 24)
;;  (require 'package)
  (add-to-list 'package-archives '("marmalade" . "https://marmalade-repo.org/packages/") t)
(package-initialize)
;;)
;; (load-file "~/.emacs.d/themes/subatomic-theme.el")
;; ( load-file "~/.emacs.d/themes/zenburn-theme.el")
;; (if (display-graphic-p)
;;    (add-hook 'after-init-hook (lambda () (load-theme 'subatomic t)))
;; (add-hook 'after-init-hook (lambda () (load-theme 'zenburn t)))
;; )
;;customize theme
(add-to-list 'custom-theme-load-path "~/.emacs.d/elpa/atom-one-dark-theme-20180215.828/")
(load-theme 'atom-one-dark t)
(if (display-graphic-p)
    (enable-theme 'atom-one-dark)
  (disable-theme 'atom-one-dark))


;;(add-to-list 'load-path "/u/ahamdi/.emacs.d/elpa/auto-complete-auctex-20140223.958/")
;;(add-to-list 'load-path "/u/ahamdi/.emacs.d/elpa/auto-complete-c-headers-20150911.2023/")

;; start auto-complete with emacs
(require 'auto-complete)
;;(global-auto-complete-mode t)
; do default config for auto-complete
(require 'auto-complete-config)
(ac-config-default)
; start yasnippet with emacs
(require 'yasnippet)
(yas-global-mode 1)

; let's define a function which initializes auto-complete-c-headers and gets called for c/c++ hooks
(defun my:ac-c-header-init ()
  (require 'auto-complete-c-headers)
  (add-to-list 'ac-sources 'ac-source-c-headers)
  (add-to-list 'achead:include-directories '"/data.local/nacer/root-6.06.08/include/")
  ;;(add-to-list 'achead:include-directories '"/Applications/Xcode.app/Contents/Developer/usr/llvm-gcc-4.2/lib/gcc/i686-apple-darwin11/4.2.1/include")
)
; now let's call this function from c/c++ hooks
(add-hook 'c++-mode-hook 'my:ac-c-header-init)
(add-hook 'c-mode-hook 'my:ac-c-header-init)

;;(require 'cedet)
;;(global-ede-mode t)
;;(require 'semantic)  ; turn on Semantic
(semantic-mode 1)
(require 'cc-mode)
(require 'semantic)

(global-semanticdb-minor-mode 1)
(global-semantic-idle-scheduler-mode 1)


(semantic-add-system-include "/data.local/nacer/root-6.06.08/include/" 'c++-mode)
(semantic-add-system-include "/data.local/nacer/root-6.06.08/include/" 'c-mode)

; let's define a function which adds semantic as a suggestion backend to auto complete
; and hook this function to c-mode-common-hook
(defun my:add-semantic-to-autocomplete() 
  (add-to-list 'ac-sources 'ac-source-semantic)
)
(add-hook 'c-mode-common-hook 'my:add-semantic-to-autocomplete)
; turn on ede mode 
(global-ede-mode 1)

; create a project for our program.
;(ede-cpp-root-project "my project" :file "~/nacer/halld_my/pimpipkmkp/"
;		      :include-path '("/data.local/nacer/root-6.06.08/include/"))

; you can use system-include-path for setting up the system header file locations.
; turn on automatic reparsing of open buffers in semantic
;(global-semantic-idle-scheduler-mode 1)

;; For c++ add paths to include directories or PreProcessor definitions, just examples, add your own as required
;;(semantic-add-system-include (concat (getenv "ROOTSYS") "/include/") 'c++-mode)
;;(add-to-list 'semantic-lex-c-preprocessor-symbol-map '("LCIO_MODE" . "1"))
;;(semantic-add-system-include "/data.local/nacer/root-6.06.08/include/")

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(column-number-mode t)
 '(cua-mode t nil (cua-base))
 '(custom-safe-themes (quote (show-paren-mode t)))
 '(show-paren-mode t)
 '(x-select-enable-clipboard t))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(default ((t (:inherit nil :stipple nil :inverse-video nil :box nil :strike-through nil :overline nil :underline nil :slant normal :weight normal :height 130 :width normal ))))
 '(scroll-bar ((t nil))))

;;Turn on hl-line:
(global-hl-line-mode 1)

;;To keep syntax highlighting in the current line:
(set-face-foreground 'highlight nil)

;;(set-foreground-color "#839496")
;;(set-background-color "#002b36")

;; open with single window
(setq inhibit-startup-screen t)
(add-hook 'emacs-startup-hook 'delete-other-windows)

;;scrollbar
(scroll-bar-mode t)
(set-scroll-bar-mode t)

;; using speedbar for quick buffer navigation.
 (global-set-key [f8] "\M-x\ speedbar")

;; maximize Emacs frame
(add-to-list 'default-frame-alist '(fullscreen . maximized))

;; disable the bell
(setq ring-bell-function 'ignore)

;; delete backup files
;;(setq make_backup_files nil)
(setq backup-directory-alist '(("." . "~/.emacs.d/backup"))
  backup-by-copying t    ; Don't delink hardlinks
  version-control t      ; Use version numbers on backups
  delete-old-versions t  ; Automatically delete excess backups
  kept-new-versions 20   ; how many of the newest versions to keep
  kept-old-versions 5    ; and how many of the old
  )


;; UTF-8 as default encoding
(set-language-environment "UTF-8")

;; Dictionary
(autoload 'ispell-get-word "ispell")
(defun lookup-word (word)
  (interactive (list (save-excursion (car (ispell-get-word nil)))))
  (eww (format "http://en.wiktionary.org/wiki/%s" word)))
(global-set-key (kbd "<f7>") 'lookup-word)
