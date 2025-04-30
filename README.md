You can't enforce it globally via config yet, but you can modify the frontend source or add a redirect script in a custom Superset image.

If you're using a custom image (which you are), inject this JS override:

js
Copy
Edit
// In your custom Superset frontend override (e.g., inside `superset-frontend`)
// Add this to your `DashboardList.jsx` or inject via template

localStorage.setItem('dashboard_list_view', 'card');  // 'card' = grid view





Go to your dashboard in edit mode.

Move the native filters bar into a horizontal row at the top of the dashboard (outside the scroll area).

Use drag-and-drop to align filters in a horizontal grid.

Publish the layout.





This is URL rewriting – possible using reverse proxy configuration (e.g., Nginx).

Solution: Update your Nginx config to rewrite URLs:

nginx
Copy
Edit
location /BI/InsightView/ {
    rewrite ^/BI/InsightView/(.*)$ /superset/dashboard/$1 break;
    proxy_pass http://your_superset_backend;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
Then, use this URL format:

ruby
Copy
Edit
https://insightshub-dev.exlservice.com/BI/InsightView/14/?native_filters_key=...
Ensure your Superset is behind this reverse proxy and doesn’t break internal routes.






Modify DashboardCard.jsx in superset-frontend to display the image.

Here’s an outline:

jsx
Copy
Edit
<Card>
  <img src={dashboard.thumbnail_url || defaultImage} />
  <CardBody>
    <CardTitle>{dashboard.title}</CardTitle>
    ...
  </CardBody>
</Card>
