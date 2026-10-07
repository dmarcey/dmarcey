# Soft-nav repro for missing nested paths

Add this file to `dmarcey/dmarcey` on `master`. Hard-load this file (or a
non-code-view page such as Issues first, then come here), open DevTools →
Network → Fetch/XHR, clear it, then click each link below.

A soft nav shows no new document request. A layout fetch shows up as a
`/_serverFn/...` request. Record its status for each link.

## Missing nested paths (the shape that used to 500 in dotcom)

- [tree: missing/nested](/dmarcey/dmarcey/tree/master/foobar/nested)
- [tree: missing/nested/deeper](/dmarcey/dmarcey/tree/master/foobar/nested/deeper)
- [blob: missing/nested file](/dmarcey/dmarcey/blob/master/foobar/nested/file.md)

## Bad ref with a nested path (the case dotcom still 500s in its own tests)

- [tree: bad ref](/dmarcey/dmarcey/tree/not-a-branch/foobar/nested)
- [blob: bad ref](/dmarcey/dmarcey/blob/not-a-branch/foobar/nested/file.md)

## Single missing segment (control, should not 500)

- [tree: missing](/dmarcey/dmarcey/tree/master/foobar)

## Different repo (owner/repo change, forces the layout to refetch)

Same-repo links reuse the cached layout, so they never fetch it. Linking to
another repo changes owner/repo, which should refetch `_file_tree_layout`.
Expect a `/_serverFn/...` request on click. A new document request means it was
a hard nav instead.

- [other repo: missing/nested](/octocat/Hello-World/tree/master/foobar/nested)
- [other repo: bad ref + nested](/octocat/Hello-World/tree/not-a-branch/foobar/nested)
- [other repo: missing nested blob](/octocat/Hello-World/blob/master/foobar/nested/file.md)
- [other repo: single missing segment (control)](/octocat/Hello-World/tree/master/foobar)
