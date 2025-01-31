


```python
import requests
import pandas as pd

# Replace with your GitLab instance details
GITLAB_URL = "https://gitlab.dev9server.com/"
ACCESS_TOKEN = "your-token"

headers = {"PRIVATE-TOKEN": ACCESS_TOKEN}

def get_groups():
    """Fetch all groups."""
    url = f"{GITLAB_URL}/api/v4/groups"
    params = {"all_available": True, "per_page": 100}
    groups = []
    while url:
        response = requests.get(url, headers=headers, params=params)
        response.raise_for_status()
        groups.extend(response.json())
        url = response.links.get("next", {}).get("url")
    return groups

def get_projects(group_id):
    """Fetch all projects for a group."""
    url = f"{GITLAB_URL}/api/v4/groups/{group_id}/projects"
    params = {"per_page": 100}
    projects = []
    while url:
        response = requests.get(url, headers=headers, params=params)
        response.raise_for_status()
        projects.extend(response.json())
        url = response.links.get("next", {}).get("url")
    return projects

def main():
    groups = get_groups()
    with pd.ExcelWriter("gitlab_groups_projects.xlsx", engine="openpyxl") as writer:
        for group in groups:
            group_name = group["name"]
            group_id = group["id"]
            projects = get_projects(group_id)

            # Collect detailed project data
            data = []
            for project in projects:
                data.append({
                    "Project Name": project["name"],
                    "Project ID": project["id"],
                    "Created At": project["created_at"],
                    "Disk Usage (MB)": project["statistics"]["repository_size"] / (1024 ** 2) if "statistics" in project else None,
                    "SSH URL": project["ssh_url_to_repo"],
                    "HTTPS URL": project["http_url_to_repo"]
                })

            # Create a DataFrame and write to a sheet
            df = pd.DataFrame(data)
            if not df.empty:
                df.to_excel(writer, sheet_name=group_name[:31], index=False)
            else:
                print(f"No projects found for group: {group_name}")

    print("Data exported to gitlab_groups_projects.xlsx")

if __name__ == "__main__":
    main()
```