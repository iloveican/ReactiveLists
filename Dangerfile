has_source_changes = !git.modified_files.grep(/Source/).empty?

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn("PR is classed as Work in Progress") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > 500

# Milestones are required to track what's included in each release
if has_source_changes && !github.pr_json['milestone'].nil?
  warn('All pull requests should have a milestone attached', sticky: false)
end

# Changelog entries are required for changes to library files
no_changelog_entry = !git.modified_files.include?("CHANGELOG.md")
if has_source_changes && no_changelog_entry && git.lines_of_code > 10
  warn("Source code changes (in APIs or behaviors) should have an entry in CHANGELOG.md.", sticky: false)
end

# Docs are regenerated when releasing
has_doc_changes = !git.modified_files.grep(/docs\//).empty?
has_doc_gen_title = github.pr_title.include? "#docgen"
if has_doc_changes && !has_doc_gen_title
  fail("Docs are regenerated when creating new releases.")
  message("Docs are generated by using [Jazzy](https://github.com/realm/jazzy). If you want to contribute, please update the [markdown guides](https://github.com/plangrid/ReactiveLists/tree/master/Guides) or doc comments.")
end

swiftlint.verbose = true
swiftlint.binary_path = './Pods/SwiftLint/swiftlint'
swiftlint.config_file = './.swiftlint.yml'
swiftlint.lint_files(inline_mode: true)
