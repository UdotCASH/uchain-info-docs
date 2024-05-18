# Customizing CSS

If you would like to customize the theme, follow these steps after a successful deployment:

1. ssh in and stop the uchaininfo service:\
   `sudo systemctl stop uchaininfo.service`\

2. Navigate to `uchaininfo/apps/block_scout_web/assets/css/theme`.\

3. Edit `_poa_variables.scss` with appropriate theme colors.\

4. Rebuild the static assets: `sudo npm run-script deploy` (from uchaininfo/apps/block\_scout\_web/assets directory).\

5. Restart the uchaininfo service:\
   `sudo systemctl start uchaininfo.service`

The uchaininfo application will now be running with the designated theme changes.\
