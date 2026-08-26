---
title: Translating a game on weblate
aliases:
  - /Translating_a_game_on_weblate
  - /Translating_a_game_on_Weblate
  - /translation/weblate
---

# Translating a game on Weblate

If you have a game, it is very useful to post it on a [Weblate](https://weblate.org/) instance. Weblate is a free software that creates a website where translators can translate software from their web browser. This makes it much easier for translators to contribute, because editing those locale files by hand can get tedious. Luanti itself is hosted on a Weblate instance.

This guide gives a **rough** (!) step-by-step guide on how to set-up Weblate for your Luanti game.

**Disclaimer**: This guide does not guide you through every click but only points to the rough steps to help you get started. Some details are left out. Don't follow this guide blindly but also think before you do. This is _not_ a foolproof guide and in later Weblate versions might not work exactly like that. When in doubt, refer to the [Weblate documentation](https://docs.weblate.org/).

## Step 1: Use the PO format

Weblate understands the PO file format natively, but not the Luanti-specific TR format (it’s just too obscure). So the first step is to make sure your game uses the gettext PO file instead of the Luanti-specific TR format (which is less flexible anyway). The PO format is supported since Luanti 5.10.0.

The unofficial [Luanti Translation Tools](https://codeberg.org/Wuzzy/Luanti_Translation_Tools) contain a tool to convert from TR to PO and back.

## Step 2: Make sure your translation works and is of high-quality

Make sure your translation system works well and is of high quality. It will save you from a lot of headache later if you do it right the first time, rather than cleaning up the mess later.

Read [Preparing Program Sources](https://www.gnu.org/software/gettext/manual/html_node/Sources.html) of the gettext manual to learn about best practices.

## Step 3: Update your POT and PO files

Make sure your locale files are all up-to-date before uploading to Weblate. You also should follow these conventions for every mod:

- All locale files follow the format `<modname>.<language>.po`
- POT files are named `<modname>.pot`

## Step 4: Create the Weblate project

Use "Add new translation project" and enter the project details. This should be pretty self-explanatory.

Note this assumes your project is already under some version source control (like Git). This is out of scope for this guide.

## Step 5: Create the first component

Pick a mod you consider to be most "central" or "core" to your game (as long it contains translatable texts). Create a component with the same name as this mod. We will call that the "core component" from now on.

## Step 6: Configure your component

Configure your component. Many fields are self-explanatory, but here's a list of important fields:

- **Translation license**: Don't forget to set one!
- **Translation flags**: Put in `placeholders:r"@[1-9]"`. This will highlight the placeholders `@1` to `@9` for the translators and also activates several useful error checkers
- **Source string bug reporting address**: Translators can send e-mail to this address if they have a problem. This will also appear in the PO/POT files. Not strictly necessary but may be helpful
- **Repository browser**: Optional. Will add useful link to point translators directly to the string's origin in the source code. The syntax depends on the code host. Replace `USER` and `PROJECT` with the correct URL parts, leave the rest intact.
  - For [Github](https://github.com/), put in `https://github.com/USER/PROJECT/blob/{{branch}}/{{filename|parentdir}}#L{{line}}`.
  - For [Codeberg](https://codeberg.org/), put in `https://codeberg.org/USER/PROJECT/src/branch/{{branch}}/{{filename|parentdir}}#L{{line}}`.
  - For others, figure it out yourself or just leave this field empty. :P

## Step 7: Create the other components

Unless your game contains only one translatable mod, you need to create components for all other translatable mods now.

You have two choices: Either you create all other components manually. Or you use an add-on to create them automatically for you.

### Step 7.1: Manual creation

This is only recommended if your game is very small and only has a few mods that don’t change very often.

For the manual creation, you just repeat steps 5 and 6 for each mod. The downside is that with any change of your mod structure, you need to manually reconfigure your Weblate project as well.

### Step 7.2: Automatic creation

This is recommended if you game has a large number of mods or the list of mods changes often.

You can use a add-on to automatically create components based on rules.

1.  Go to the core component you created in step 5, then go to its add-ons page.
2.  Add the add-on "Component discovery"
3.  Configure this add-on with the following settings:
    - **Regular expression to match translation files against**: `mods/(?P<component>[a-zA-Z0-9_]*)/locale/(?P=component)\.(?P<language>[a-z]*)\.po`. This finds the PO files in your game
    - **File format**: gettext PO
    - **Customize the component name**: `{{ component }}`
    - **Define the base file for new translations**: `mods/{{ component }}/locale/{{ component }}.pot`. This finds the POT files.
4.  Hit "Save". Carefully review what Weblate now tells you. It should show you a list of new components along with associated PO and POT files. Exception: It may tell you that nothing will be added for your core component because those files already exist. This is normal.
5.  If everything looks OK, confirm this list and hit "Save" again

If it worked, Weblate will work in the background to add the remaining components. This add-on is useful as it will also automatically add a new component when a new translatable mod appears in your project.

## Step 8: Refinement

Final steps to do to make your translation project translator-friendly:

- Briefly click through the components and look if they contain strings, just to test if everything is there
- Add translator instructions for the entire project (in project settings)
- Go through project settings and settings for your core component to customize things that interest you

## Step X: Maintenance

If you have completed step 8, your project is now officially set up.

But step X is the step that never ends: Maintenance. You want to be able to import new strings into Weblate, as well as pulling strings from Weblate into your project again.

With the current setup, you have to do it manually. If you push to your project, Weblate should receive the new strings automatically soon. To get the strings from Weblate, you need to pull manually from the URL in the project information page.

You can optionally set up a push URL in Weblate to automate this but this is out of scope for this guide. Refer to the [Weblate documentation](https://docs.weblate.org/) for this.

## See also

- [Translating Mods and Games](/for-creators/translation/mods-and-games)
