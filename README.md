The reason for the Release download instead of just the files dumped into the repo like orignally, github messed with the formatting. Apologies <3 

For intial install you will want to remove or comment out the Jitsi Authentication block, you can then add this back in after your intial install has finished, otherwise it breaks the install

added some extra tag blocks you can comment them out as per your use case

Before I forget, you can use the faster "just" command with the connection local syntax after the initial install via ansible playbook command, it would look like this "just install-all -c=local" "just setup-all -c=local".
