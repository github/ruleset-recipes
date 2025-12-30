#!/usr/bin/env bash
#
# destroy_github_all.sh
# - Dry-run by default: daftar semua resource yang akan dihapus.
# - Untuk benar-benar menjalankan penghapusan, atur EXECUTE="YES_I_UNDERSTAND_DELETE_PERMANENTLY"
# - Requires: curl, jq
#
# Usage (dry-run):
#   GITHUB_TOKEN="ghp_xxx" ./destroy_github_all.sh
# To perform destructive actions:
#   GITHUB_TOKEN="ghp_xxx" EXECUTE="YES_I_UNDERSTAND_DELETE_PERMANENTLY" ./destroy_github_all.sh
#
set -euo pipefail

# Safety checks
if ! command -v curl >/dev/null 2>&1; then
  echo "Error: curl diperlukan. Install curl dan ulangi." >&2
  exit 2
fi
if ! command -v jq >/dev/null 2>&1; then
  echo "Error: jq diperlukan. Install jq dan ulangi." >&2
  exit 2
fi

: "${GITHUB_TOKEN:?GITHUB_TOKEN environment variable must be set (PAT with appropriate scopes)}"

API="https://api.github.com"
AUTH_HEADER="Authorization: token ${GITHUB_TOKEN}"
PER_PAGE=100

# Execution guard: must set EXECUTE env to the exact phrase to actually delete.
EXECUTE="${EXECUTE:-}"
EXECUTE_PHRASE="YES_I_UNDERSTAND_DELETE_PERMANENTLY"

is_execute_run() {
  [ "$EXECUTE" = "$EXECUTE_PHRASE" ]
}

echo "===== GitHub Mass-Delete Script (dry-run by default) ====="
echo "This script will operate on resources your token has permission to modify."
echo "If you REALLY want to proceed with permanent deletion, set:"
echo "  EXECUTE=\"$EXECUTE_PHRASE\""
echo

# Helper: paginated GET
paged_get() {
  local url="$1"
  local page=1
  while :; do
    resp=$(curl -sS -H "$AUTH_HEADER" "$url&per_page=$PER_PAGE&page=$page")
    if [ "$(echo "$resp" | jq 'length')" -eq 0 ]; then
      break
    fi
    echo "$resp"
    page=$((page+1))
  done
}

# 1) List user info (for safety)
user_login=$(curl -sS -H "$AUTH_HEADER" "$API/user" | jq -r .login)
if [ "$user_login" = "null" ] || [ -z "$user_login" ]; then
  echo "Gagal mengambil informasi user. Periksa token." >&2
  exit 3
fi
echo "Authenticated as: $user_login"
echo

# Confirm interactive prompt before any destructive run (extra guard)
if is_execute_run; then
  echo "EXECUTION MODE: WILL PERFORM DELETES (permanent)."
  echo "FINAL CHECK: Ketik nama akun Anda EXACTLY untuk melanjutkan: $user_login"
  read -r confirmname
  if [ "$confirmname" != "$user_login" ]; then
    echo "Nama tidak cocok. Dibatalkan."
    exit 1
  fi
else
  echo "DRY-RUN mode: tidak akan melakukan penghapusan. Untuk mengaktifkan penghapusan, set EXECUTE to $EXECUTE_PHRASE"
  echo
fi

# FUNCTIONS TO COLLECT AND (optionally) DELETE

# Delete repositories owned by the authenticated user
handle_repos() {
  echo "=== Checking repositories owned by $user_login ==="
  repos_json=$(curl -sS -H "$AUTH_HEADER" "$API/user/repos?type=owner&per_page=$PER_PAGE")
  repos=$(echo "$repos_json" | jq -r '.[].full_name')
  if [ -z "$repos" ]; then
    echo "(no owner repos found)"
    return
  fi

  echo "Repositories found (owner):"
  echo "$repos" | sed 's/^/ - /'
  echo

  if is_execute_run; then
    for r in $repos; do
      echo "Deleting repository: $r"
      resp_code=$(curl -s -o /dev/null -w "%{http_code}" -X DELETE -H "$AUTH_HEADER" "$API/repos/$r")
      if [ "$resp_code" -eq 204 ]; then
        echo "  OK: $r deleted"
      else
        echo "  WARNING: Failed to delete $r, HTTP $resp_code"
      fi
    done
  fi
}

# Delete gists
handle_gists() {
  echo "=== Checking Gists ==="
  gists_json=$(curl -sS -H "$AUTH_HEADER" "$API/gists?per_page=$PER_PAGE")
  gist_ids=$(echo "$gists_json" | jq -r '.[].id')
  if [ -z "$gist_ids" ]; then
    echo "(no gists found)"
    return
  fi
  echo "Gists found:"
  echo "$gist_ids" | sed 's/^/ - /'
  echo
  if is_execute_run; then
    for gid in $gist_ids; do
      echo "Deleting gist $gid"
      resp_code=$(curl -s -o /dev/null -w "%{http_code}" -X DELETE -H "$AUTH_HEADER" "$API/gists/$gid")
      if [ "$resp_code" -eq 204 ]; then
        echo "  OK: gist $gid deleted"
      else
        echo "  WARNING: Failed to delete gist $gid, HTTP $resp_code"
      fi
    done
  fi
}

# Delete public SSH keys for authenticated user
handle_user_keys() {
  echo "=== Checking SSH public keys for user ==="
  keys_json=$(curl -sS -H "$AUTH_HEADER" "$API/user/keys")
  key_ids=$(echo "$keys_json" | jq -r '.[].id')
  if [ -z "$key_ids" ]; then
    echo "(no user SSH keys found)"
    return
  fi
  echo "User SSH key IDs:"
  echo "$key_ids" | sed 's/^/ - /'
  echo
  if is_execute_run; then
    for kid in $key_ids; do
      echo "Deleting SSH key id=$kid"
      resp_code=$(curl -s -o /dev/null -w "%{http_code}" -X DELETE -H "$AUTH_HEADER" "$API/user/keys/$kid")
      if [ "$resp_code" -eq 204 ]; then
        echo "  OK: key $kid deleted"
      else
        echo "  WARNING: Failed to delete key $kid, HTTP $resp_code"
      fi
    done
  fi
}

# For each repo: delete deploy keys, webhooks, secrets, releases, artifacts
handle_repo_level() {
  echo "=== Scanning each owner repo for deploy-keys, webhooks, secrets and releases ==="
  repos_json=$(curl -sS -H "$AUTH_HEADER" "$API/user/repos?type=owner&per_page=$PER_PAGE")
  repos=$(echo "$repos_json" | jq -r '.[].full_name')
  if [ -z "$repos" ]; then
    echo "(no owner repos found)"
    return
  fi
  for r in $repos; do
    owner=$(echo "$r" | cut -d'/' -f1)
    repo=$(echo "$r" | cut -d'/' -f2)
    echo "-> Repo: $r"

    # Deploy keys
    dk_json=$(curl -sS -H "$AUTH_HEADER" "$API/repos/$owner/$repo/keys")
    dk_ids=$(echo "$dk_json" | jq -r '.[].id')
    if [ -n "$dk_ids" ]; then
      echo "   Deploy keys:"
      echo "$dk_ids" | sed 's/^/    - /'
      if is_execute_run; then
        for id in $dk_ids; do
          echo "    Deleting deploy key $id"
          resp_code=$(curl -s -o /dev/null -w "%{http_code}" -X DELETE -H "$AUTH_HEADER" "$API/repos/$owner/$repo/keys/$id")
          [ "$resp_code" -eq 204 ] && echo "      OK" || echo "      WARN HTTP $resp_code"
        done
      fi
    fi

    # Webhooks (hooks)
    hooks_json=$(curl -sS -H "$AUTH_HEADER" "$API/repos/$owner/$repo/hooks")
    hook_ids=$(echo "$hooks_json" | jq -r '.[].id')
    if [ -n "$hook_ids" ]; then
      echo "   Webhooks:"
      echo "$hook_ids" | sed 's/^/    - /'
      if is_execute_run; then
        for hid in $hook_ids; do
          echo "    Deleting hook $hid"
          resp_code=$(curl -s -o /dev
