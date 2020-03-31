!!! info "Providing user with `root` permission"
    If you are running NGINX as a user that does not have `root` permission, add this user to the `wallarm` group using the following command:
>
    ```term
    # usermod -aG wallarm &lt;user_name&gt;
    ```
    
    where `<user_name>` is the name of the user without `root` permission.