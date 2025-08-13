==========================
Contribute to the Workshop
==========================

Adding a New Page
=================

To add a new page to the workshop:

1. Create a new `.rst` file in the appropriate directory within the repository.

   - For example, if the page is related to Docker, place it under the `docker/`
     directory.

2. Update the `index.rst` file under workshop directory to include a reference to your new page.

   - Add the relative path to your `.rst` file under the appropriate section.

3. Ensure your `.rst` file follows the ``reStructuredText`` format and includes
   a title and content.

4. Commit your changes and push them to the repository.

Automatic Build and Deployment
==============================

The workshop uses GitHub Actions to automatically build and publish updates to
GitHub Pages. Once your changes are pushed to the repository:

1. The GitHub Actions workflow will generate the HTML files for the workshop.
2. The updated workshop will be published to GitHub Pages, making your new page
   accessible online.

No additional steps are required for deployment. Ensure your `.rst` file is
correctly formatted to avoid build errors.
