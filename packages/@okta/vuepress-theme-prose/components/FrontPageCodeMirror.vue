<template>
  <textarea
    disabled
    ref="codemirror"
    name="codemirror"
    id="codemirror"
  ></textarea>
</template>

<script>
import "codemirror/lib/codemirror.css";

export default {
  name: "FrontPageCodeMirror",
  mounted: async function() {
    // load side effects for code mirror
    await import("codemirror/mode/javascript/javascript.js");
    // load codemirror
    const module = await import("codemirror");
    const CodeMirror = module.default;

    this.codemirror = CodeMirror.fromTextArea(
      this.$refs.codemirror,
      this.cmOptions
    );
    this.codemirror.setValue(this.widgetCode);
    this.codemirror.setOption("mode", "javascript");
    this.codemirror.setOption("readOnly", "nocursor");
  },
  data() {
    return {
      widgetCode: `
// Uses okta-signin-widget version -=OKTA_REPLACE_WITH_WIDGET_VERSION=-

var widget = new OktaSignIn({
  baseUrl: "{{yourOktaDomain}}",
  logo: "/sites/all/themes/developer/owb/alliance.png",
  i18n: {
    en: {
      "primaryauth.title": "Alliance Authentication",
      "primaryauth.submit": "Sign In"
    },
  },
});
widget.renderEl({
  el: "#widget-container"
});
      `.trim(),
      cmOptions: {
        lineNumbers: true,
        mode: "text/javascript",
        styleActiveLine: true,
        readOnly: true,
        indentUnit: 4,
      },
    };
  },
  beforeDestroy: function() {
    this.destroyCm();
  },
  methods: {
    destroyCm() {
      const element = this.codemirror.doc.cm.getWrapperElement();
      element && element.remove && element.remove();
    },
  },
};
</script>
