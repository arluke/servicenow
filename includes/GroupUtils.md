### Script:
```
var GroupUtils = Class.create();
GroupUtils.prototype = Object.extendsObject(AbstractAjaxProcessor, {

  getUsersInGroup: function(group) {
    // Pass in group common name as 'group'
    // Returns comma-delimited user sys_id values string
    var members;
    
    var grName = new GlideRecord('sys_user_group');
    grName.addQuery('name', group);
    grName.setLimit(1);
    grName.Query();

    var grMembers = new GlideRecord('sys_user_grmember');
    grMembers.addQuery('group', grName.getUniqueValue());
    grMembers.Query();

    while (grMembers.next) {
      members += grMembers.user;
      if (grMembers.next) {
        members += ","
      }
    }
    return members;
  }

  currentUserGroupCheck() {
    // Client Script Callable
    // Pass in group common name as 'sysparm_group'
    // Returns true if currrent user is in group
    var group = this.getParameter('sysparm_group');

    var grName = new GlideRecord('sys_user_group');
    grName.addQuery('name', group);
    grName.setLimit(1);
    grName.Query();

    var grMember = new Gliderecord('sys_user_grmember');
    grMember.addQuery('group', grName.getUniqueValue();
    grMember.addQuery('user', gs.getUserID());
    grMember.Query();

    if (grMember.next) {
      return true;
    } else {
      return false;
    }
  }

  type: 'GroupUtils'
});
```

### GroupUtils.getUsersInGroup()
Useful for filtering reference fields in catalog items via Advanced Reference Qualifier:
```
javascript: "sysidIN" + new GroupUtils().getUsersInGroup(groupname);
```

### GroupUtils.currentUserGroupCheck()
Client-side implementation of user.isMemberOf().  Useful in client scripts to where groups are utilized instead of roles.
