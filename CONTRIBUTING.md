1.  GitHub account and GitHub-compatible git tools of your choice.

    GitHub provides a setup guide is [here](https://docs.github.com/en/get-started/git-basics/set-up-git).

2. Clone the `astemes-lunit` repo and the `astemes-string-diff-utils` submodule.

    If your tool does not automatically clone the `astemes-string-diff-utils` submodule, execute the following command from within your local `astemes-lunit` folder:
    ```shell
    git submodule update --init --recursive
    ```

3.  Development stack:

    LUnit is developed in LabVIEW 2020 in order to maintain package compatibility back to that version.
    
    Install the latest version of the LUnit package via VIPM: https://www.vipm.io/package/astemes_lib_lunit/

    Open the `LUnit Framework.lvproj` project under `source`.
    
    > Note: The VIPM package installs the LUnit project provider and copies of LUnit's core VIs into `vi.lib`. This means you can run the full provider functionality with the `LUnit Framework.lvproj` (including executing LUnit's own self-tests), but be aware that the provider will load different copies of the core VIs than what is in the project context. This can be a bit of a hassle if you are trying to update/exercise core framework classes and VIs, but unfortunately it is a limitation of LV provider development.
    >
    > FYI, some of the LVClasses are also intentionally renamed in the development project vs. the packaged/installed files; e.g. `LUnit Test Case.lvclass` in the dev project becomes just `Test case.lvclass` when packaged and installed. This pattern also helps avoid *some* of the namespace confusion related to how the provider and core files are installed, but it also means that tests created and/or run via the development VIs can work a bit differently.