<template>
  <v-container class="pa-3" style="position: relative;">
    <prompt
      :prompt="prompt"
      :textInput="textInput"
      :disabled="disabled"
    ></prompt>
    <text-input
      :value="textInput"
      @input="textInput = $event"
      :prompt="prompt"
      :disabled="disabled"
      ref="textInput"
      style="position:absolute;z-index: -1;left: 0; top:0 ;"
    ></text-input>
  </v-container>
</template>

<script>
import Prompt from "./Prompt.vue";
import TextInput from "./TextInput.vue";

export default {
  props: ["difficulty", "testDuration"],

  components: {
    Prompt,
    TextInput,
  },

  data() {
    return {
      prompt: "",
      textInput: "",
      disabled: false,

      prompts: {
        easy: [
          "Two members of the 1984 class of Jefferson High School are chairing a group of 18 to look for a resort for the 20-year class reunion. A lovely place 78 miles from the city turns out to be the best. It has 254 rooms and a banquet hall to seat 378. It has been open 365 days per year since opening on May 30, 1926. They will need 450 to reserve the resort. Debbie Holmes was put in charge of buying 2,847 office machines for the entire firm. Debbie visited more than 109 companies in 35 states in 6 months. She will report to the board today in Room 2784 at 5 p.m. The board will consider her report about those 109 firms and recommend the top 2 or 3 brands to purchase. Debbie must decide before August 16. Lynn Greene said work started on the project March 27, 2003. The 246 blueprints were mailed to the office 18 days ago. The prints had to be 100 percent accurate before they were acceptable. The project should be finished by May 31, 2025. At that time there will be 47 new condominiums, each having at least 16 rooms. The building will be 25 stories.",
          "Being human makes us susceptible to the onset of feelings. The role of these emotions varies. Some of them are useful while others may be harmful. The use of social media for self-expression has reached a point that it makes us feel we can say anything. This begins when we see people expressing anything and everything that come to mind. When we see everyone else voicing their likes and dislikes, their irritations and desires we tend to imitate what they do. And because many engage in this, we think that it is normal and healthy. However, when we get used to unbridled self-expression, we come to believe that all feelings are valid. We become convinced that in real life, we should also act on our emotions and our impulses. Using social media this way erodes our ability to regulate our actions and reactions. To illustrate, when something small irritates us we think that it's okay to feel this way. But isn't it better to foster one's patience and resilience instead of immediately complaining? Or when we develop an attraction to someone despite that person being in a relationship, and because social media has conditioned us that all feelings can be expressed, we tend to think that acting on this attraction is okay. Not all feelings deserve expression. We find ourselves creating our own problems when we let our present emotions control our actions.",
          "An ever-growing number of complex and rigid rules plus hard-to-cope-with regulations are now being legislated from state to state. Key federal regulations were formulated by the FDA, FTC, and the CPSC. Each of these federal agencies serves a specific mission. One example: Laws sponsored by the Office of the Fair Debt Collection Practices prevent an agency from purposefully harassing clients in serious debt. The Fair Packaging and Labeling Act makes certain that protection from misleading packaging of goods is guaranteed to each buyer of goods carried in small shops as well as in large supermarkets. Products on the market must reveal the names of all ingredients on the label. Language must be in clear and precise terms that can be understood by everyone. This practice is very crucial for the lives of many people. It is prudent that we recall that the FDA specifically requires that all goods are pure, safe, and wholesome. The FDA states that all goods be produced under highly sanitary conditions. Drugs must be completely safe and must also be effective for their stated purpose. This policy applies to cosmetics that must be both safe and pure. Individuals are often totally unappreciative of the FDA's great dedication.",
        ],
        medium: [
          "One study examining 30 subjects, of varying different styles and expertise, has found minimal difference in typing speed between touch typists and self-taught hybrid typists. According to the study, 'The number of fingers does not determine typing speed... People using self-taught typing strategies were found to be as fast as trained typists... instead of the number of fingers, there are other factors that predict typing speed... fast typists... keep their hands fixed on one position, instead of moving them over the keyboard, and more consistently use the same finger to type a certain letter.' To quote doctoral candidate Anna Feit: 'We were surprised to observe that people who took a typing course, performed at similar average speed and accuracy, as those that taught typing to themselves and only used 6 fingers on average' (Wikipedia)",
          "Historically, the fundamental role of pharmacists as a healthcare practitioner was to check and distribute drugs to doctors for medication that had been prescribed to patients. In more modern times, pharmacists advise patients and health care providers on the selection, dosages, interactions, and side effects of medications, and act as a learned intermediary between a prescriber and a patient. Pharmacists monitor the health and progress of patients to ensure the safe and effective use of medication. Pharmacists may practice compounding; however, many medicines are now produced by pharmaceutical companies in a standard dosage and drug delivery form. In some jurisdictions, pharmacists have prescriptive authority to either independently prescribe under their own authority or in collaboration with a primary care physician through an agreed upon protocol.",
        ],
        hard: [
          "The Master of Business Administration (MBA or M.B.A.) degree originated in the United States in the early 20th century when the country industrialized and companies sought scientific approaches to management. The core courses in an MBA program cover various areas of business such as accounting, applied statistics, business communication, business ethics, business law, finance, managerial economics, management, entrepreneurship, marketing and operations in a manner most relevant to management analysis and strategy. Most programs also include elective courses and concentrations for further study in a particular area, for example accounting, finance, and marketing. MBA programs in the United States typically require completing about sixty credits, nearly twice the number of credits typically required for degrees that cover some of the same material such as the Master of Economics, Master of Finance, Master of Accountancy, Master of Science in Marketing and Master of Science in Management, The MBA is a terminal degree and a professional degree. Accreditation bodies specifically for MBA programs ensure consistency and quality of education. Business schools in many countries offer programs tailored to full-time, part-time, executive (abridged coursework typically occurring on nights or weekends) and distance learning students, many with specialized concentrations.",
          "A teacher's professional duties may extend beyond formal teaching. Outside of the classroom teachers may accompany students on field trips, supervise study halls, help with the organization of school functions, and serve as supervisors for extracurricular activities. In some education systems, teachers may have responsibility for student discipline.",
          "The first personnel management department started at the National Cash Register Co. in 1900. The owner, John Henry Patterson, organized a personnel department to deal with grievances, discharges and safety, and training for supervisors on new laws and practices after several strikes and employee lockouts. During the 1970s, companies experienced globalization, deregulation, and rapid technological change which caused the major companies to enhance their strategic planning and focus on ways to promote organizational effectiveness. This resulted in developing more jobs and opportunities for people to show their skills which were directed to effective applying employees toward the fulfillment of individual, group, and organizational goals. Many years later the major/minor of human resource management was created at universities and colleges also known as business administration.",
        ],
      },
    };
  },

  computed: {
    consecutiveErrors() {
      let last = this.textInput.length - 1,
        secondToLast = this.textInput.length - 2;

      return (
        this.textInput[last] !== this.prompt[last] &&
        this.textInput[secondToLast] !== this.prompt[secondToLast]
      );
    },
  },

  methods: {
    selectPrompt(difficulty) {
      let randomIndex = Math.floor(Math.random() * 3);

      return (this.prompt = this.prompts[difficulty][randomIndex]);
    },
    checkForConsecutiveErrors() {
      if (this.consecutiveErrors) {
        this.disabled = true;
        this.textInput = this.textInput.slice(0, this.textInput.length - 1);

        return this.reenable();
      }
    },
    reenable() {
      setTimeout(() => {
        this.disabled = false;
        this.$nextTick(() => this.$refs.textInput.$el.focus());
      }, 250);
    },
  },

  watch: {
    textInput: "checkForConsecutiveErrors",
  },

  created() {
    return this.selectPrompt(this.difficulty);
  },

  mounted() {
    return this.$refs.textInput.$el.focus();
  },
};
</script>

<style>
</style>